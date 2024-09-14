# -*- mode: ruby -*-
# vim: set ft=ruby :
# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  prometheus: {
    box_name: 'ubuntu/22.04',
    vm_name: 'prometheus',
    # :public => {:ip => "10.10.10.1", :adapter => 1},
    net: [
      # ip, adapter, netmask, virtualbox_intnet
      ['192.168.255.1', 2, '255.255.255.252', 'router-net'],
      ['192.168.50.10', 8, '255.255.255.0']
    ]
  }

}
ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'
Vagrant.configure('2') do |config|
  MACHINES.each do |boxname, boxconfig|
    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.host_name = boxconfig[:vm_name]

      box.vm.provider 'virtualbox' do |v|
        v.memory = 768
        v.cpus = 1
      end

      boxconfig[:net].each do |ipconf|
        box.vm.network('private_network', ip: ipconf[0], adapter: ipconf[1], netmask: ipconf[2],
                                          virtualbox__intnet: ipconf[3])
      end

      box.vm.network 'public_network', boxconfig[:public] if boxconfig.key?(:public)

      box.vm.provision 'shell', inline: <<-SHELL
          mkdir -p ~root/.ssh
          cp ~vagrant/.ssh/auth* ~root/.ssh
      SHELL

      # if boxconfig[:vm_name] == "office2Server"
      #  box.vm.provision "ansible" do |ansible|
      #   ansible.playbook = "configure.yaml"
      #   ansible.inventory_path = "hosts"
      #  ansible.host_key_checking = "false"
      #   ansible.limit = "all"
      #  end
      # end
    end
  end
end
