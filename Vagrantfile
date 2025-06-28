# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # variáveis do grupo para fácil substituição
  nome1 = "Nilson"
  nome2 = "Wellington"
  matricula1_ultimos_digitos = "02"
  matricula2_ultimos_digitos = "31"
  matricula_xy = "21"


  # Configurações comuns a todas as VMs
  config.vm.box = "debian/bookworm64"
  config.ssh.insert_key = false # Geração de chaves ssh desativada

  # Servidor de Arquivos (arq)
  config.vm.define "arq" do |arq|
    arq.vm.hostname = "arq.#{nome1}.#{nome2}.devops"
    arq.vm.network "private_network", ip: "192.168.56.1#{matricula1_ultimos_digitos}"
    arq.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.linked_clone = true # Utilização de clones (linked_clone)

      # Discos adicionais (3 discos de 10 GB cada)
      (1..3).each do |i|
        disk_name = "arq_disk_#{i}.vdi"
        disk_path = File.join(Dir.home, ".vagrant.d", "boxes", "debian-bookworm64", disk_name)

        unless File.exist?(disk_path)
          vb.customize [
            "createhd",
            "--filename", disk_path,
            "--size", 10 * 1024 # 10 GB em MB
          ]
        end
        vb.customize ["storageattach", :id, "--storagectl", "SATA Controller", "--port", i + 1, "--device", 0, "--type", "hdd", "--medium", disk_path]
      end
    end
  end 

  # Servidor de Banco de Dados (db)
  config.vm.define "db" do |db|
    db.vm.hostname = "db.#{nome1}.#{nome2}.devops"
    db.vm.network "private_network", type: "dhcp" # Endereço IP na rede privada obtido via DHCP
    db.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.linked_clone = true # Utilização de clones (linked_clone)
    end
  end 

  # Servidor de Aplicação (app)
  config.vm.define "app" do |app|
    app.vm.hostname = "app.#{nome1}.#{nome2}.devops"
    app.vm.network "private_network", type: "dhcp" # Endereço IP na rede privada obtido via DHCP
    app.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.linked_clone = true # Utilização de clones (linked_clone)
    end
  end 

  # Host Cliente (cli)
  config.vm.define "cli" do |cli|
    cli.vm.hostname = "cli.#{nome1}.#{nome2}.devops"
    cli.vm.network "private_network", type: "dhcp" # Endereço IP na rede privada obtido via DHCP
    cli.vm.provider "virtualbox" do |vb|
      vb.memory = "1024" # Memória RAM: 1024 MB
      vb.linked_clone = true # Utilização de clones (linked_clone)
    end
  end


  # Configuração de provisionamento (Ansible)
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible/site.yml"
    # Passando as variáveis definidas no Vagrantfile para o Ansible
    ansible.extra_vars = {
      nome1: nome1,
      nome2: nome2,
      matricula1_ultimos_digitos: matricula1_ultimos_digitos,
      matricula2_ultimos_digitos: matricula2_ultimos_digitos,
      matricula_xy: matricula_xy
    }
    # Para se conectar como usuário vagrant padrão do Debian
    # Desabilitar a verificação de host key (útil em ambientes de PoC)
    ansible.host_key_checking = false
  end

end
