Vagrant.configure("2") do |config|
    server_ip = "192.168.10.30"
    config.vm.provider "virtualbox" do |vb|
	vb.memory = 1024
    	vb.cpus = 1
    	vb.name = "mongo-ubuntu-bionic"
    end

    config.vm.box = "ubuntu/bionic64"
    config.vm.network "public_network", ip: "#{server_ip}"
    config.vm.synced_folder "./config", "/configs"
    #config.vm.synced_folder ".", "/vagrant", disabled: true

    config.vm.provision "shell", inline: "apt-get --yes update || true"
	
	#Atualiza os pacotes e faz a instalação do MongoDB
    config.vm.provision "shell",
    	inline: "wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add - && \
				apt-get install gnupg && \
				wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add - && \
				echo 'deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse' | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list && \
				sudo apt-get -y update && \
				sudo apt-get install -y mongodb-org && \
				echo 'mongodb-org hold' | sudo dpkg --set-selections && \
				echo 'mongodb-org-server hold' | sudo dpkg --set-selections && \
				echo 'mongodb-org-shell hold' | sudo dpkg --set-selections && \
				echo 'mongodb-org-mongos hold' | sudo dpkg --set-selections && \
				echo 'mongodb-org-tools hold' | sudo dpkg --set-selections && \
				sudo sed -i 's/127.0.0.1/#{server_ip}/g' /etc/mongod.conf && \ 
				sudo systemctl enable mongod && \
				sudo systemctl start mongod"
end
