# -*- mode: ruby -*-
# vi: set ft=ruby :

vms = {
  'master' => {'memory' => '512', 'cpus' => 1, 'ip' => '10'},
  'node01' => {'memory' => '512', 'cpus' => 1, 'ip' => '20'},
#   'node02' => {'memory' => '512', 'cpus' => 1, 'ip' => '30'},
#   'node03' => {'memory' => '512', 'cpus' => 1, 'ip' => '40'}

}

Vagrant.configure('2') do |config|

  config.vm.box = './debian10'
  config.vm.box_check_update = false

  vms.each do |name, conf|
    config.vm.define "#{name}" do |m|
      m.vm.hostname = "#{name}.swarm.local"
      m.vm.network 'private_network', ip: "192.168.56.#{conf['ip']}"

      m.vm.provider 'virtualbox' do |vb| # VirtualBox
        vb.memory = conf['memory']
        vb.cpus = conf['cpus']
      end
      m.vm.provider 'libvirt' do |lv| # Libvirt
        lv.memory = conf['memory']
        lv.cpus = conf['cpus']
        lv.cputopology :sockets => 1, :cores => conf['cpus'], :threads => '1'
      end

      m.vm.provision "shell", inline: <<-'SHELL'
        apt-get update
        apt-get install -y apt-transport-https ca-certificates curl software-properties-common gnupg-agent
        mkdir -p /etc/apt/keyrings
        curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
        echo \
        "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
        $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
        apt-get update
        apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
      SHELL

    end

  end

end