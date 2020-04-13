# -*- mode: ruby -*-
# vi: set ft=ruby  :

machines = {
  "master" => {"memory" => "1024", "cpu" => "2", "ip" => "100", "image" => "ubuntu/bionic64"},
  "node01" => {"memory" => "1024", "cpu" => "2", "ip" => "101", "image" => "ubuntu/bionic64"},
  "node02" => {"memory" => "1024", "cpu" => "2", "ip" => "102", "image" => "centos/7"}
}

Vagrant.configure("2") do |config|

  machines.each do |name, conf|
    config.vm.define "#{name}" do |machine|
      machine.vm.box = "#{conf["image"]}"
      machine.vm.hostname = "#{name}.caiodelgado.dev"
      machine.vm.network "private_network", ip: "10.10.10.#{conf["ip"]}"
      machine.vm.provider "virtualbox" do |vb|
        vb.name = "#{name}"
        vb.memory = conf["memory"]
        vb.cpus = conf["cpu"]
        vb.customize ["modifyvm", :id, "--groups", "/Docker-Lab"]
      end
      machine.vm.provision "shell", path: "docker.sh"
      if "#{conf["image"]}" == "centos/7"
        machine.vm.provision "shell", inline: "sudo systemctl start docker && sudo systemctl enable docker"
      end
      if "#{name}" == "master"
        machine.vm.provision "shell", path: "master.sh"
      else
        machine.vm.provision "shell", path: "worker.sh"
      end
    end
  end
end
