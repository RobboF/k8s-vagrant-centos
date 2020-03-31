Vagrant.configure("2") do |config|
  config.vm.define :master do |master|
    master.vm.box = "robbof/centos7-k8s"
    master.vm.box_version = "0.6.0"
    master.vm.network "private_network", ip: "10.0.0.10"
    master.vm.hostname = "master"
  end
  config.vm.define :worker1 do |worker|
    worker.vm.box = "robbof/centos7-k8s"
    worker.vm.box_version = "0.6.0"
    worker.vm.network "private_network", ip: "10.0.0.11"
    worker.vm.hostname = "worker"
  end
  config.vm.define :worker2 do |worker2|
    worker2.vm.box = "robbof/centos7-k8s"
    worker2.vm.box_version = "0.6.0"
    worker2.vm.network "private_network", ip: "10.0.0.12"
    worker2.vm.hostname = "worker2"
  end
  config.vm.define :etcd do |etcd|
    etcd.vm.box = "robbof/centos7-k8s"
    etcd.vm.box_version = "0.6.0"
    etcd.vm.network "private_network", ip: "10.0.0.13"
    etcd.vm.hostname = "etcd"
  end
  config.vm.define :ingress do |ingress|
    ingress.vm.box = "robbof/centos7-k8s"
    ingress.vm.box_version = "0.6.0"
    ingress.vm.network "private_network", ip: "10.0.0.13"
    ingress.vm.hostname = "ingress"
  end
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
    vb.memory = 2048
    vb.cpus = 2
  end
end
