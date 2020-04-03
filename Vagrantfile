servers = [
  {
    :name => "haproxy",
    :type => "loadbalancer",
    :box => "robbof/centos7-k8s",
    :version => "0.7.0",
    :ip => "10.0.0.10",
    :RAM => "2048",
    :CPU => "2"
  },
  {
    :name => "master",
    :type => "master",
    :box => "robbof/centos7-k8s",
    :version => "0.7.0",
    :ip => "10.0.0.11",
    :RAM => "3072",
    :CPU => "2"
  },
  {
    :name => "control1",
    :type => "control",
    :box => "robbof/centos7-k8s",
    :version => "0.7.0",
    :ip => "10.0.0.12",
    :RAM => "3072",
    :CPU => "2"
  },
  {
    :name => "control2",
    :type => "control",
    :box => "robbof/centos7-k8s",
    :version => "0.7.0",
    :ip => "10.0.0.13",
    :RAM => "3072",
    :CPU => "2"
  },
  {
    :name => "worker1",
    :type => "worker",
    :box => "robbof/centos7-k8s",
    :version => "0.7.0",
    :ip => "10.0.0.14",
    :RAM => "3072",
    :CPU => "2"
  },
  {
    :name => "worker2",
    :type => "worker",
    :box => "robbof/centos7-k8s",
    :version => "0.7.0",
    :ip => "10.0.0.15",
    :RAM => "3072",
    :CPU => "2"
  }
]

Vagrant.configure("2") do |config|
  servers.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.box = opts[:box]
      config.vm.box_version = opts[:version]
      config.vm.network "private_network", ip: opts[:ip]
      config.vm.hostname = opts[:name]
      config.vm.provider "virtualbox" do |v|
        v.name = opts[:name]
        v.customize ["modifyvm", :id, "--memory", opts[:RAM]]
        v.customize ["modifyvm", :id, "--cpus", opts[:CPU]]
      end
      config.vm.provision "shell", env: {"MASTER" => opts[:ip]}, inline: "sed -i \"s/KUBELET_EXTRA_ARGS=/KUBELET_EXTRA_ARGS=--node-ip=$MASTER/g\" /etc/sysconfig/kubelet"
      case opts[:type]
      when 'master'
        config.vm.provision "shell", env: {"MASTER" => opts[:ip]}, inline: $provisionMaster
      when 'loadbalancer'
        config.vm.provision "shell", env: {"MASTER" => opts[:ip]}, inline: $provisionHAProxy
        servers.each do |server|
          if server[:type] == "master" || server[:type] == "control"
            config.vm.provision "shell", env: {"IP" => server[:ip], "NAME" => server[:name]}, inline: "echo \"    server $NAME $IP:6443 check fall 3 rise 2\" >> /etc/haproxy/haproxy.cfg" 
          end
        end
        config.vm.provision "shell", inline: "setsebool haproxy_connect_any on" 
        config.vm.provision "shell", inline: "systemctl start haproxy" 
        config.vm.provision "shell", inline: "systemctl enable haproxy" 
      when 'control'
      else
        config.vm.provision "shell", env: {"WORKER" => opts[:ip]}, inline: $provisionNode
      end
    end
  end
end

$provisionMaster = <<-SCRIPT
cp /vagrant/files/haproxy.bak /vagrant/files/haproxy.cfg
kubeadm init --pod-network-cidr $MASTER/24 --apiserver-advertise-address $MASTER --control-plane-endpoint 10.0.0.10:6443 --upload-certs
kubeadm token create --print-join-command > /vagrant/kubeadm_join_cmd.sh
chmod +x /vagrant/kubeadm_join_cmd.sh
cp /etc/kubernetes/admin.conf /vagrant/config
mkdir -p ~/.kube && cp /etc/kubernetes/admin.conf ~/.kube/config
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
echo "copy config to local .kube" 
SCRIPT
$provisionNode = <<-SCRIPT
/vagrant/kubeadm_join_cmd.sh
SCRIPT
$provisionHAProxy = <<-SCRIPT
yum -y install haproxy
mv /etc/haproxy/haproxy.cfg{,.back}
cp /vagrant/files/haproxy.bak /etc/haproxy/haproxy.cfg
sed -i "s/x.x.x.x/$MASTER/g" /etc/haproxy/haproxy.cfg
SCRIPT

# TODO
# create host loop for HAProxy
# env for IP
