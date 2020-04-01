servers = [
  {
    :name => "master",
    :type => "master",
    :box => "robbof/centos7-k8s",
    :version => "0.7.0",
    :ip => "10.0.0.10",
    :RAM => "3072",
    :CPU => "2"
  },
  {
    :name => "worker",
    :type => "worker",
    :box => "robbof/centos7-k8s",
    :version => "0.7.0",
    :ip => "10.0.0.11",
    :RAM => "3072",
    :CPU => "2"
  },
  {
    :name => "worker2",
    :type => "worker",
    :box => "robbof/centos7-k8s",
    :version => "0.7.0",
    :ip => "10.0.0.12",
    :RAM => "3072",
    :CPU => "2"
  },
  {
    :name => "etcd",
    :type => "etcd",
    :box => "robbof/centos7-k8s",
    :version => "0.7.0",
    :ip => "10.0.0.13",
    :RAM => "3072",
    :CPU => "2"
  },
  {
    :name => "ingress",
    :type => "ingress",
    :box => "robbof/centos7-k8s",
    :version => "0.7.0",
    :ip => "10.0.0.14",
    :RAM => "3072",
    :CPU => "2"
  },
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
      if opts[:type] == "master"
        config.vm.provision "shell", inline: $provisionMaster
      else
        config.vm.provision "shell", inline: $provisionNode
      end
    end
  end
end

$provisionMaster = <<-SCRIPT
kubeadm init --pod-network-cidr 10.0.0.10/24 --apiserver-advertise-address 10.0.0.10
kubeadm token create --print-join-command >> /vagrant/kubeadm_join_cmd.sh
chmod +x /vagrant/kubeadm_join_cmd.sh
cp /etc/kubernetes/admin.conf /vagrant/config
echo "copy config to local .kube" 
SCRIPT
$provisionNode = <<-SCRIPT
/vagrant/kubeadm_join_cmd.sh
SCRIPT
