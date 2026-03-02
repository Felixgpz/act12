# vi: set ft=ruby :

################################
#### Definició de variables ####
################################

IMATGE_BOX_NODES = "debian/bookworm64"
NOM_VIRTUALBOX = "pj9f4a12-fepuza"  
HOSTNAME_SISTEMA = "pj9f4a12"        
NOM_DOMINI = "clotfje.net"
MEMORIA_RAM = 2048
NUM_CPUS = 3
TARGETA_XARXA = "Realtek RTL8852BE WIFI 6 802.11ax PCle Adapter"

Vagrant.configure("2") do |config|
   
   config.vm.define NOM_VIRTUALBOX do |node|
      node.vm.box = IMATGE_BOX_NODES
      node.vm.hostname = HOSTNAME_SISTEMA 
      node.vm.network "public_network", bridge: TARGETA_XARXA
      
      node.vm.provider "virtualbox" do |v|
         v.name = NOM_VIRTUALBOX
         v.memory = MEMORIA_RAM
         v.cpus = NUM_CPUS
         v.customize ['modifyvm', :id, '--clipboard', 'bidirectional']
      end

      ### Aprovisionament de Docker i eines ###
      node.vm.provision "shell", inline: <<-SHELL
         sudo apt-get update -y
         sudo apt-get install -y net-tools whois aptitude git zip unzip curl
         sudo apt-get -y install apt-transport-https ca-certificates curl gnupg
         
         curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker.gpg
         echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker.gpg] \
         https://download.docker.com/linux/debian bookworm stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
         
         sudo apt-get -y update
         sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
         sudo gpasswd -a vagrant docker
      SHELL
   end
end