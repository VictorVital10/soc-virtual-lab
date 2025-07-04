# 🛡️ SOC Virtual Lab em Cloud - Projeto com Ferramentas Open Source

Este repositório documenta o desenvolvimento de um **laboratório SOC (Security Operations Center)** funcional, construído na **Google Cloud Platform (GCP)** com foco em ferramentas **open source** e aplicação de conceitos práticos de **cibersegurança, resposta a incidentes e infraestrutura em nuvem**.

---

## 🎯 Objetivo

Simular um ambiente de **Segurança Operacional completo**, com foco na **detecção, automação e investigação de incidentes de segurança**, reforçando o aprendizado prático sobre as tecnologias utilizadas em ambientes reais de defesa cibernética.

---

## 🧰 Tecnologias Utilizadas

| Componente       | Função                                                                 |
|------------------|------------------------------------------------------------------------|
| **Wazuh (SIEM)** | Implementação de regras, análise de logs, geração e envio de alertas via webhook |
| **Shuffle (SOAR)** | Orquestração de playbooks para resposta automatizada a eventos críticos |
| **IRIS (DFIR)** | Recebimento de alertas para análise e investigação forense              |
| **Docker**        | Implantação de todos os serviços em contêineres para garantir isolamento e portabilidade |
| **GCP Firewall**  | Controle de acesso às portas e serviços expostos                       |
| **Kali Linux**    | Simulação de ataques: brute force, port scan e exploração              |
| **Agentes**       | VMs Windows e Linux enviando logs de eventos reais                     |

---

## 🧱 Infraestrutura

- Total de **6 instâncias** na GCP:
  - 1x Wazuh Server (SIEM)
  - 1x IRIS (DFIR)
  - 1x Shuffle (SOAR)
  - 1x Kali Linux (Red Team)
  - 1x Agente Linux
  - 1x Agente Windows

- Todas as integrações foram realizadas via **HTTPS e autenticação por token**.
- O acesso externo foi controlado via **firewall nativo da GCP** com regras específicas por serviço e origem.
- Os contêineres foram gerenciados com **Docker** em cada VM.

---

## ⚔️ Simulações Realizadas

- Execução de comandos administrativos (sudo)
- Alteração e criação de arquivos no agente Linux
- Execução de binários suspeitos no Windows
- Port scan e brute force via Kali Linux
- Alertas enviados automaticamente ao IRIS
- Respostas orquestradas via Shuffle

---

## 📸 Imagens do Projeto

Veja imagens do ambiente, alertas e dashboards na pasta `/images` deste repositório.

---

## 📘 Requisitos e Dicas

- A conta GCP precisa estar com a cobrança ativa
- Para simular alertas, utilize atividades anômalas nos agentes
- Não se esqueça de configurar o firewall da GCP antes de iniciar os testes

---

## 🚀 Próximos Passos

- Integração com fontes externas de Threat Intelligence (ex: AbuseIPDB, VirusTotal)  
- Criação de dashboards customizados no Kibana  
- Expansão para um ambiente híbrido com rede interna simulada  

---

## 📎 Link para o Post no LinkedIn

👉 [Acessar publicação](https://www.linkedin.com/in/seu-perfil-exemplo) 

---

## 👤 Autor

**Victor Araujo Vital**  

📌 LinkedIn: [https://www.linkedin.com/in/victor-vital-6b6761275/](https://www.linkedin.com/in/victor-vital-6b6761275/)

---