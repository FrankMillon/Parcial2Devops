# Vagrantfile
Vagrant.configure("2") do |config|
    # Configuración de la primera máquina virtual con Apache y la carpeta 'web1'
    config.vm.define "web1" do |web1|
      web1.vm.box = "ubuntu/bionic64"
      web1.vm.network "private_network", ip: "192.168.56.101"
      web1.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      web1.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y apache2
        systemctl enable apache2
        systemctl start apache2
      SHELL
      web1.vm.synced_folder "./web1", "/var/www/html" # Directorio compartido con 'web1'
    end
  
    # Configuración de la segunda máquina virtual con Apache y la carpeta 'web2'
    config.vm.define "web2" do |web2|
      web2.vm.box = "ubuntu/bionic64"
      web2.vm.network "private_network", ip: "192.168.56.102"
      web2.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      web2.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y apache2
        systemctl enable apache2
        systemctl start apache2
      SHELL
      web2.vm.synced_folder "./web2", "/var/www/html" # Directorio compartido con 'web2'
    end
  
    # Configuración de la tercera máquina virtual con Nginx
    config.vm.define "loadbalancer" do |loadbalancer|
      loadbalancer.vm.box = "ubuntu/bionic64"
      loadbalancer.vm.network "private_network", ip: "192.168.56.103"
      loadbalancer.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      loadbalancer.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y nginx
        cat <<EOF > /etc/nginx/conf.d/load_balancer.conf
        upstream web_servers {
            server 192.168.56.101;
            server 192.168.56.102;
        }
        server {
            listen 80;
            location / {
                proxy_pass http://web_servers;
            }
        }
        EOF
        systemctl restart nginx
      SHELL
    end
  end
  