Vagrant.configure("2") do |config|
config.vm.box = "madebian12"
config.vm.box_url = "https://github.com/charliewalker-art/boxe-image-debian/releases/download/v1.0.0/package.box"

  # ── Nom de la machine ──────────────────────────────────────────────────────
  config.vm.hostname = "apimobile"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "apimobile-automation"
    vb.memory = "2048"
    vb.cpus   = 2 
  end

  config.vm.network "private_network", ip: "192.168.57.20"

# ── Provisioning Shell ─────────────────────────────────────────────────────
  config.vm.provision "shell", inline: <<-SHELL
    export DEBIAN_FRONTEND=noninteractive
    apt-get update
   
    apt-get install -y ansible python3-debian 

  
    # Changer le mot de passe root
    echo "root:charlie" | chpasswd

    # Création de l'utilisateur ca (si n'existe pas déjà)
    if ! id "charlie" &>/dev/null; then
      useradd -m -s /bin/bash charlie
      echo "charlie:1234" | chpasswd
    fi

    # Ajout de charlie au groupe sudo
    usermod -aG sudo charlie
  SHELL

  # Étape 1 : Installer Docker
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "conf-docker/install_docker.yml"
    ansible.verbose = "v"
  end

    # Étape 2 : Configurer SSH pour Ansible
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "conf-docker/setup_ssh_ansible.yml"
    ansible.verbose = "v"
  end

    # Étape 3 : Déployer l'application
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "conf-docker/deploy_app.yml"
    ansible.verbose = "v"
  end
end


   
 

  

 


