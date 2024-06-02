Vagrant.configure("2") do |config|
  ## VM for target deployments
  config.vm.define "target", autostart: false do |target|
    target.vm.box = "mprokopov/ubuntu-target"
    target.vm.network "forwarded_port", guest: 4444, host: 4444
    target.vm.network "private_network", ip: "192.168.105.3"
    target.vm.synced_folder '.', '/vagrant', disabled: true # avoid requirement for vboxfs
    target.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
    end
  end

  config.vm.define "jenkins" do |target|
    target.vm.box = "mprokopov/ubuntu-jenkins"

    # jenkins port
    target.vm.network "forwarded_port", guest: 8080, host: 8080
    # docker port
    target.vm.network "forwarded_port", guest: 2375, host: 2375

    target.vm.network "private_network", ip: "192.168.105.2"

    target.vm.synced_folder '.', '/vagrant', disabled: true

    target.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
    end
  end

  config.vm.define "k8s", autostart: false do |target|
    target.vm.box = "mprokopov/ubuntu-k8s"
    target.vm.network "forwarded_port", guest: 6443, host: 6443

    target.vm.network "private_network", ip: "192.168.105.4"
    target.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
    end
  end
end

