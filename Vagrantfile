VAGRANTFILE_API_VERSION = "2"
VAGRANT_DISABLE_VBOXSYMLINKCREATE = "1"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
# Use same SSH key for each machine
config.ssh.insert_key = false
config.vm.box_check_update = false
config.vm.define "ipa" do |ipa|
  ipa.vm.box = "centos/7"
  ipa.vm.provision :shell, :inline => "sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config; sudo systemctl restart sshd;", run: "always"
  ipa.vm.network "forwarded_port", guest: 80, host: 8081
  ipa.vm.network "forwarded_port", guest: 443, host: 8445
  ipa.vm.hostname = "ipa.example.com"
  ipa.vm.network "private_network", ip: "192.168.55.5"
  ipa.vm.provider :virtualbox do |ipa|
    ipa.customize ['modifyvm', :id,'--memory', '3072']
    end
  ipa.vm.provision "ansible" do |ansible|
    ansible.playbook = 'playbooks/ipa.yml'
  end
end
  
config.vm.define "system1" do |system1|
  system1.vm.box = "puppetlabs/centos-7.0-64-nocm"
  system1.vm.network "forwarded_port", guest: 80, host: 8082
  system1.vm.network "forwarded_port", guest: 443, host: 8446
  system1.vm.hostname = "system1.example.com"
  system1.vm.network "private_network", ip: "192.168.55.6"
  system1.vm.provision :shell, :inline => "sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config; sudo systemctl restart sshd;", run: "always"

  system1.vm.provider :virtualbox do |system1|
    system1.customize ['modifyvm', :id,'--memory', '1024']
    end
  system1.vm.provision "ansible" do |ansible|
    ansible.playbook = 'playbooks/system1.yml'
  end
end
config.vm.define "system2" do |system2|
  system2.vm.box = "puppetlabs/centos-7.0-64-nocm"
  system2.vm.network "forwarded_port", guest: 80, host: 8083
  system2.vm.network "forwarded_port", guest: 443, host: 8447
  system2.vm.hostname = "system2.example.com"
  system2.vm.network "private_network", ip: "192.168.55.7"
  system2.vm.provision :shell, :inline => "sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config; sudo systemctl restart sshd;", run: "always"

  system2.vm.provider :virtualbox do |system2|
    system2.customize ['modifyvm', :id,'--memory', '1024']
    end
  system2.vm.provision "ansible" do |ansible|
    ansible.playbook = 'playbooks/system2.yml'
  end
end
end