# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
	config.vbguest.auto_update = false
	cluster_nodes = %w[cp1 node1 node2 node3]
	groups = {
		'cp1' => ['cp1'],
		'node1' => ['node1'],
		'node2' => ['node2'],
		'node3' => ['node3'],
	}

	cluster_nodes.each_with_index do |node, index|
		config.vm.define node do |kube_cluster|
			kube_cluster.vm.box = "bento/ubuntu-18.04"
			kube_cluster.vm.hostname = "#{node}"
			kube_cluster.vm.network 'private_network', ip: "192.168.58.#{index + 100}"
			kube_cluster.vm.provider 'virtualbox' do |vb|
				cpus = 8
				vb.cpus = cpus / 2
				vb.customize ['modifyvm', :id, '--groups', '/KUBE']
				vb.customize ['modifyvm', :id, '--nestedpaging', 'on']
				vb.customize ['modifyvm', :id, '--largepages', 'on']
				vb.customize ['modifyvm', :id, '--ioapic', 'on'] if cpus > 1
				if node.start_with?('node')
					vb.memory = 6144
				else
					vb.memory = 4096
				end
				if index == groups.size - 1
					kube_cluster.vm.provision 'setup', type: "ansible" do |ansible|
						ansible.playbook = "cluster_setup.yml"
						ansible.limit = "all"
						ansible.groups = groups
						ansible.verbose = 'vv'
						ansible.extra_vars = {
							nfs_server: "192.168.58.45",
							nfs_path: "/data/k8"
							}
					end
				end
			end
		end
	end
end
