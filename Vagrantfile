N = 2
NETWORK = "192.168.77"
ANSIBLE_GROUPS = {
  "masters" => ["node-1"],
  "workers" => ["node-[2:#{N}]"],
  "cluster:children" => ["masters", "workers"]
}

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.vm.box = "generic/ubuntu2204"
  config.vm.provider "libvirt" do |v|
    v.memory = 2048
    v.cpus = 2
  end


  (1..N).each do |i|
    config.vm.define "node-#{i}" do |node|
      node.vm.network "private_network", ip: "#{NETWORK}.#{i + 9}"
      node.vm.hostname = "node-#{i}"
        
      if i == N
        node.vm.provision "ansible" do |ansible|
          # ansible.verbose = "vvv"
          ansible.playbook = "playbook.yml"
          ansible.compatibility_mode = "2.0"
          ansible.groups = ANSIBLE_GROUPS
          ansible.limit = "all"
        end
      end
    end
  end
end
