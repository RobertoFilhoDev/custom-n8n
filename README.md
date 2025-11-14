# Integração Wazuh → n8n via Webhooks  
Automação de envio de alertas Wazuh para workflows do n8n

Este repositório contém dois arquivos que implementam uma integração personalizada entre o **Wazuh Manager** e o **n8n**, permitindo o envio de alertas em tempo real via HTTP POST no formato JSON.

A integração foi projetada para:

- enviar alertas automaticamente ao n8n usando webhooks;
- suportar qualquer tipo de regra Wazuh com nível ≥ 3;
- normalizar e enviar os eventos no formato esperado pela automação;
- funcionar nativamente dentro de `/var/ossec/integrations/`.

  
---

# 📌 1. Requisitos

- Wazuh Manager 4.x ou superior  
- Python 3 embutido no Wazuh (localizado em `/var/ossec/framework/python/bin/python3`)
- Módulo `requests` instalado no Python do Wazuh  
- n8n acessível via webhook público ou interno

---

# 📌 2. Instalação

## 📍 2.1. Copiar os arquivos para o diretório de integrações

No servidor Wazuh Manager:

```bash
sudo cp custom-n8n.sh /var/ossec/integrations/custom-n8n.sh
sudo cp custom-n8n.py /var/ossec/integrations/custom-n8n.py
sudo chmod +x /var/ossec/integrations/custom-n8n.sh

sudo nano /var/ossec/etc/ossec.conf

<integration>
    <name>custom-n8n</name>
    <hook_url>https://SEU-WEBHOOK-N8N/webhook/ID</hook_url>
    <level>3</level>
    <alert_format>json</alert_format>
</integration>
sudo systemctl restart wazuh-manager
