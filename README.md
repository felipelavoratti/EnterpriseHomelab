# 🧠 EnterpriseHomelab

Homelab projetado para aplicar e demonstrar fundamentos de infraestrutura, monitoramento e orquestração com foco em aprendizado e portfólio técnico.

---

## 🛠️ Objetivo

Criar um ambiente replicável para estudos e testes utilizando VMs provisionadas com Vagrant e VirtualBox, monitoradas via Zabbix e visualizadas em Grafana. Também integra alertas com bot no Telegram e prepara o terreno para testes com Docker/Kubernetes.

---

## 🔧 Infraestrutura Atual

Todas as VMs sobem localmente em modo `bridge` para comunicação real com dispositivos da rede.

| Nome da VM       | IP               | Função                         | Recursos      |
|------------------|------------------|--------------------------------|---------------|
| ZabbixServer     | 192.168.100.160  | Servidor Zabbix com banco de dados | 2GB RAM / 1 CPU |
| Grafana          | 192.168.100.161  | Visualização gráfica dos dados  | 1GB RAM / 1 CPU |
| ZabbixAgent      | 192.168.100.162  | Agente monitorado via Zabbix   | 512MB RAM / 1 CPU |

---

## 📁 Estrutura de Pastas

```plaintext
homelab-projeto/
├── README.md
├── tsplus01/
│   ├── Vagrantfile
│   └── notas-configuracao.md
├── monitor01/
│   ├── Vagrantfile
│   └── setup-zabbix.md
├── grafana01/
│   ├── Vagrantfile
│   └── dashboards.md
├── docker01/
│   ├── Vagrantfile
│   ├── docker-compose.yml
│   └── k8s/
│       └── minikube-notes.md
├── telegram-bot/
│   └── zabbix-integration.md
├── screenshots/
│   └── dashboards.png
└── LICENSE


tsplus01 - Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.define "tsplus01" do |vm|
    vm.vm.box = "gusztavvargadr/windows-server"
    vm.vm.hostname = "tsplus01"

    # Substitui private_network por public_network (bridge)
    vm.vm.network "public_network", bridge: "Realtek PCIe GBE Family Controller", use_dhcp_assigned_default_route: true

    vm.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
    end
  end
end


---
monitor01 - Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.define "monitor01" do |vm|
    vm.vm.box = "ubuntu/focal64" # ou outro box
    vm.vm.hostname = "monitor01"

    # Remove private_network e usa modo bridge
    vm.vm.network "public_network", bridge: "Realtek PCIe GBE Family Controller", use_dhcp_assigned_default_route: true

    vm.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
    end

    vm.vm.provision "shell", inline: <<-SHELL
      sudo apt update
      sudo apt install -y zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-agent
    SHELL
  end
end

----
grafana01 - Vagrantfile

Vagrant.configure("2") do |config|
  config.vm.define "grafana01" do |vm|
    vm.vm.box = "ubuntu/focal64" # ou outro box
    vm.vm.hostname = "grafana01"

    # Remove private_network e usa modo bridge
    vm.vm.network "public_network", bridge: "Realtek PCIe GBE Family Controller", use_dhcp_assigned_default_route: true

    vm.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
end
    vm.vm.provision "shell", inline: <<-SHELL
      sudo apt update
      sudo apt install -y grafana
    SHELL
  end
end


---
docker01 - Vagrantfile

Vagrant.configure("2") do |config|
  config.vm.define "docker01" do |vm|
    vm.vm.box = "ubuntu/focal64" # ou outro box
    vm.vm.hostname = "docker01"

    # Remove private_network e usa modo bridge
    vm.vm.network "public_network", bridge: "Realtek PCIe GBE Family Controller", use_dhcp_assigned_default_route: true

    vm.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
end
    vm.vm.provision "shell", inline: <<-SHELL
      echo "Provisionamento inicial"
    SHELL
 
    vm.vm.provision "shell", inline: <<-SHELL
      sudo apt update
      sudo apt install -y docker.io
      sudo usermod -aG docker vagrant
      curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
      sudo install minikube-linux-amd64 /usr/local/bin/minikube
    SHELL
  end
end





