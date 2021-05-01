# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
	cluster_nodes = %w[cp1 node1 node2 node3]
	groups = {
		'cp1' => ['cp1'],
		'node1' => ['node1'],
		'node2' => ['node2'],
		'node3' => ['node3']
	}

	cluster_nodes.each_with_index do |node, index|
		config.vm.define node do |kube_cluster|
			kube_cluster.vm.box = "bento/ubuntu-18.04"
			kube_cluster.vm.hostname = "c1-#{node}"
			kube_cluster.vm.network 'private_network', ip: "192.168.58.#{index + 100}", name: 'vboxnet2'
			kube_cluster.vm.provider 'virtualbox' do |vb|
				cpus = 4
				vb.cpus = cpus / 2
				vb.customize ['modifyvm', :id, '--nestedpaging', 'on']
				vb.customize ['modifyvm', :id, '--largepages', 'on']
				vb.customize ['modifyvm', :id, '--ioapic', 'on'] if cpus > 1
				if node.start_with?('node')
					vb.memory = 2048
				else
					vb.memory = 3074
				end
				if index == groups.size - 1
					kube_cluster.vm.provision 'setup', type: "ansible" do |ansible|
						ansible.playbook = "cluster_setup.yml"
						ansible.limit = "all"
						ansible.groups = groups
						ansible.verbose = 'vv'
					end
				end
			end
		end
	end
end