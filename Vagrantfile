# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"
    
  config.vm.provision "ansible" do |ansible|
    #ansible.verbose = "vvv"
    ansible.playbook = "playbook.yml"
    ansible.become = "true"
    ansible.limit = "all"
    ansible.host_key_checking = "false"
    #ansible.inventory_path = "/inventory"
    ansible.groups = {
           "webservers" => ["web"],
           "rsyslogservers" => ["rsyslog"],
           "elkservers" => ["elk"]
        }
  end
  
  config.vm.provider "virtualbox" do |v|
        v.memory = 512
        v.cpus = 1
  end

  config.vm.define "web" do |web|
     web.vm.network "private_network", ip: "192.168.56.10", virtualbox__intnet: "net1"
     web.vm.hostname = "web"
     web.vm.network "forwarded_port", guest: 80, host: 80
  end

  config.vm.define "log" do |log|
    log.vm.network "private_network", ip: "192.168.56.15", virtualbox__intnet: "net1"
    log.vm.hostname = "rsyslog"
  end

  config.vm.define "elk" do |elk|
    elk.vm.network "private_network", ip: "192.168.56.12", virtualbox__intnet: "net1"
    elk.vm.hostname = "elk"
    elk.vm.network "forwarded_port", guest: 5601, host: 5601
    config.vm.provider :virtualbox do |vb|
       vb.customize ["modifyvm", :id, "--memory", "2048"]
       vb.customize ["modifyvm", :id, "--cpus", "1"]
   end
  end   
	
    config.vm.provision :shell do |s|
      s.inline = 'mkdir -p ~root/.ssh; cp ~vagrant/.ssh/auth* ~root/.ssh'
    end	
	   
end
