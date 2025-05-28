# Arquitetura do Laboratório SOC na Google Cloud Platform (GCP)

## Introdução à Google Cloud Platform (GCP)

A Google Cloud Platform (GCP) é a poderosa plataforma de serviços em nuvem da Google, que oferece infraestrutura acessível, escalável, flexível e segura para o desenvolvimento, implantação e gerenciamento de aplicações e serviços. A escolha da GCP para este laboratório se deu principalmente pela praticidade e acessibilidade que a plataforma oferece para quem está iniciando na área de cibersegurança. A GCP é bem flexível nesse sentido disponibilizando um crédito de aproximadamente U$300 para novos usuários, que pode ser utilizado livremente durante um período de 3 meses.

## Por que escolher GCP para o laboratório SOC

- **Acesibilidade:** Como havia citado, a GCP se destaca como uma excelente escolha para quem deseja desenvolver projetos como esse, especialmente pela acessibilidade que oferece. A plataforma possuí uma política bastante generosa com novos usuários, oferecendo créditos gratuitos que podem ser utilizados por até 90 dias, o que permite explorar diversos serviços sem custos, sendo ideal para quem deseja aprender, testar e construir projetos práticos.
- **Flexibilidade:** Ampla gama de serviços gerenciados que facilitam a criação e manutenção do ambiente.
- **Escalabilidade:** Possibilidade de ajustar recursos conforme a demanda, permitindo simular qualquer tipo de ambiente.
- **Segurança:** Ferramentas nativas para controle de acesso, monitoramento e proteção da infraestrutura. 

## Infraestrtura usada

- **Máquinas Virtuais (VMs):** instancias que hospedam o Wazuh Manager, agentes Windows e Linux, o Shuffle SOAR e o IRIS DFIR.
- **Redes Virtuais (VPC):** configuradas para garantir isolamento e controle do tráfego e maior segurança na comunicação entre as VMs.
- **Firewalls:** regras aplicadas para proteger as VMs, limitando acesso às portas essenciais e prevenindo tráfego não autorizado.
- **Storage:** armazenamento persistente de logs, configurações e dados gerados pelos serviços.

## Descrição das Máquinas Virtuais

As principais VMs criadas para o laboratório são:

- **Wazuh Manager:** responsável pela coleta, análise e correlação de eventos de segurança, funcionando com o núcleo do SIEM.
- **Agente Linux:** máquina que simulda um endpoint Linux monitorado pelo Wazuh.
- **Agente Windows:** endpoint Windows configurado para envio de logs e alertas ao Wazuh.
- **Shuffle SOAR:** plataforma para automatização de respostas a incidentes e orquestração de ações.
- **IRIS DFIR:** ferramenta para análise forense e resposta a incidetes digitais.

## Topologia da Arquitetura

![Diagrama da arquitetura do laboratório SOC](../images/topologia.png)

*Figura 1 – Diagrama simplificado da arquitetura do laboratório SOC na GCP.*

## Segurança da Infraestrutura

Para garantir a infraestrutura básica da infraestrutura na GCP, foram aplicadas as seguintes medidas:

- Configuração de regras de firewall que restringem o acesso apenass aos serviços e portas necessários para o funcionamento do laboratório.
- Utilização de contas e permissões baseadas em papéis (IAM) para limitar privilégios.
- Monitoramento inicial do tráfego e dos logs para detectar possíveis anomalias.

---

Esta arquitetura fornece uma base realista para o desenvolvimento de habilidades práticas envolvendo segurança, infraestrutura e nuvem, garantindo sempre as melhores práticas de infraestrutura segura. 