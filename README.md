🧠 EnterpriseHomelab
Homelab projetado para aplicar e demonstrar fundamentos de infraestrutura, monitoramento e orquestração, com foco em aprendizado técnico, documentação prática e portfólio profissional.

🛠️ Objetivo
Construir um ambiente replicável de laboratório local utilizando máquinas virtuais provisionadas via Vagrant e VirtualBox. O projeto integra ferramentas de monitoramento (Zabbix + Grafana), orquestração (Docker e Minikube), simulação de produção com Windows Server (TSPlus) e alertas automatizados com bot no Telegram.

🔧 Infraestrutura Atual
Todas as VMs sobem localmente em modo bridge, permitindo comunicação real entre os serviços e dispositivos da rede.

Nome da VM	IP	Função	Recursos
monitor01	192.168.100.141	Servidor Zabbix com banco de dados	2GB RAM / 1 CPU
grafana01	192.168.100.147	Visualização dos dados monitorados	1GB RAM / 1 CPU
docker01	192.168.100.148	Ambiente de containers Docker/Kubernetes	2GB RAM / 2 CPUs
tsplus01	192.168.100.157	Windows Server com TSPlus (simulação)	2GB RAM / 2 CPUs
ZabbixAgent	192.168.100.162	Agente monitorado pela Zabbix	512MB RAM / 1 CPU
📦 Componentes e Tecnologias
Zabbix: coleta de métricas e monitoramento de rede

Grafana: dashboards visuais e triggers customizadas

Docker & Minikube: orquestração e simulação de containers

TSPlus: acesso remoto e ambiente corporativo simulado

Telegram Bot: alertas de eventos e falhas via webhook

🚀 Como subir o ambiente
Clone o repositório:

bash
git clone https://github.com/seu-usuario/EnterpriseHomelab.git
cd EnterpriseHomelab
Inicialize cada VM separadamente:

bash
cd monitor01 && vagrant up
cd grafana01 && vagrant up
cd docker01 && vagrant up
cd tsplus01 && vagrant up
Acesse os serviços:

Zabbix: http://192.168.100.141/zabbix

Grafana: http://192.168.100.147:3000

RDP: mstsc para 192.168.100.157

Docker/Kubernetes: via vagrant ssh docker01

📡 Monitoramento Ativo
O Zabbix está configurado para monitorar:

Dispositivos na rede local (TV, PCs, roteador)

A própria VM Windows Server

Infraestrutura de containers via agentes

Triggers com envio de alertas via Telegram Bot
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





