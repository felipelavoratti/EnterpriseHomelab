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
C:\VMS\Homelab\
├── ZabbixServer\     # Contém Vagrantfile e scripts do servidor Zabbix
├── Grafana\          # Vagrantfile da VM com Grafana
├── ZabbixAgent\      # Vagrantfile e configs do agente
