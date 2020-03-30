Vagrant.configure("2") do |config|
  config.vm.define :master do |master|
    master.vm.box = "centos/7"
    master.vm.network "private_network", ip: "10.0.0.10"
    master.vm.hostname = "master"
    master.vm.box = "centos/7"
    master.vm.provision "file", source: "./kubernetes.repo", destination: "/tmp/kubernetes.repo"
    master.vm.provision "shell", inline: <<-SHELL
      sudo cp /tmp/kubernetes.repo /etc/yum.repos.d/kubernetes.repo
      yum -y update
      yum -y install kubelet kubeadm 
    SHELL
  end
  config.vm.define :worker1 do |worker|
    worker.vm.box = "centos/7"
    worker.vm.network "private_network", ip: "10.0.0.11"
    worker.vm.hostname = "worker"
    worker.vm.box = "centos/7"
    worker.vm.provision "file", source: "./kubernetes.repo", destination: "/tmp/kubernetes.repo"
    worker.vm.provision "shell", inline: <<-SHELL
      sudo cp /tmp/kubernetes.repo /etc/yum.repos.d/kubernetes.repo
      yum -y update
      yum -y install kubelet kubeadm 
    SHELL
  end
  config.vm.define :worker2 do |worker2|
    worker2.vm.box = "centos/7"
    worker2.vm.network "private_network", ip: "10.0.0.12"
    worker2.vm.hostname = "worker2"
    worker2.vm.box = "centos/7"
    worker2.vm.provision "file", source: "./kubernetes.repo", destination: "/tmp/kubernetes.repo"
    worker2.vm.provision "shell", inline: <<-SHELL
      sudo cp /tmp/kubernetes.repo /etc/yum.repos.d/kubernetes.repo
      yum -y update
      yum -y install kubelet kubeadm 
    SHELL
  end
  config.vm.define :etcd do |etcd|
    etcd.vm.box = "centos/7"
    etcd.vm.network "private_network", ip: "10.0.0.13"
    etcd.vm.hostname = "etcd"
    etcd.vm.box = "centos/7"
    etcd.vm.provision "file", source: "./kubernetes.repo", destination: "/tmp/kubernetes.repo"
    etcd.vm.provision "shell", inline: <<-SHELL
      sudo cp /tmp/kubernetes.repo /etc/yum.repos.d/kubernetes.repo
      yum -y update
      yum -y install kubelet kubeadm 
    SHELL
  end
  config.vm.define :ingress do |ingress|
    ingress.vm.box = "centos/7"
    ingress.vm.network "private_network", ip: "10.0.0.13"
    ingress.vm.hostname = "ingress"
    ingress.vm.box = "centos/7"
    ingress.vm.provision "file", source: "./kubernetes.repo", destination: "/tmp/kubernetes.repo"
    ingress.vm.provision "shell", inline: <<-SHELL
      sudo cp /tmp/kubernetes.repo /etc/yum.repos.d/kubernetes.repo
      yum -y update
      yum -y install kubelet kubeadm 
    SHELL
  end
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
  end
end
