## Visão Geral

> Nesta etapa, o foco principal foi validar o funcionamento de cada um dos componentes do laboratório SOC após suas respectivas instalações e configurações.

O objetivo dos testes foi garantir que todos os serviços estão se comunicando corretamente, que os agentes estão enviando eventos, que o SIEM está processando os alertas e que as integrações com as demais ferramentas (como o IRIS e o Shuffle) estão funcionando conforme o esperado.

---

## Testes de Conectividade

O primeiro passo antes de iniciar os testes de funcionalidades foi validar a conectividade entre todas as VMs na GCP.

Realizei os seguintes procedimentos:

- Teste de **ping** entre os servidores para garantir comunicação de rede
- Teste de acesso remoto via **SSH** (para instâncias Linux) e **RDP** (para a instância Windows).
- Validação de acesso às interfaces web (Wazuh Dashboard, IRIS e Shuffle).

**Exemplo de comando de testes de ping:**

``` bash
ping <IP-do-destino>
```
<!-- Inserir imagem: ping_test.png -->
<!-- Imagem X - Teste de conectividade entre as VMs usando o comando ping. -->

## Testes de Comunicação entre Agentes e Wazuh Manager

Após validar a conectividade, o próximo passo foi confirmar a comunicação entre os agentes e o Wazuh Manager.

### Testes realizados:

- Verificação do status dos agentes no Dashboard do Wazuh.
- Geração de eventos simples para confirmar a coleta de logs.

#### Exemplo de evento gerado no agente Linux:

``` bash
sudo su
```

<!-- Inserir imagem: wazuh_alert_privilege_escalation.png -->
<!-- Imagem X - Alerta de escalonamento de privilégio gerado após execução docomando 'sudo su' no agente Linux -->

Esse comando simpes foi suficiente para gerar um evento de escalonamento de privilégio, o qual apareceu no Dashboard.

#### Exemplo de evento gerado no agente Windows:

- Abertura de aplicações administrativas.
- Alterações em arquivos de sistema.

Esses eventos também apareceram como alertas no Wazuh.

## Testes de Geração de Alertas Personalizados

Para validar o pipeline de detecção e resposta, realizei alguns testes manuais de geração de alertas.

#### Exemplo: Criação de arquivo suspeito no agente Linux

``` bash
sudo touch /tmp/teste_alerta.txt
```
<!-- Inserir imagem: wazuh_alert_file_creation.png -->
<!-- Figura X - Alerta gerado no Wazuh após a criação de um arquivo suspeito no agente Linux. -->

Na imagem é possível ver o alerta gerado no Wazuh com base nas regras pré-configuradas.

#### Exemplo: Execução de comandos administrativos no Windows
- Execução do Prompt de Comando como administrador.
- Instalação e remoção de programas de testes.

Todos os eventos foram devidamente capturados e reportados ao Wazuh.

## Testes de Integração: Wazuh - IRIS

Um dos testes mais importantes foi validar a integração entre o Wazuh e o IRIS, garantindo que os alertas gerados no SIEM fossem encaminhados corretamente para o IRIS para posterior análise e investigação forense.

### Procedimento realizado:

#### 1 - Configuração de um Webhook no Wazuh:
- Definição do IP do IRIS como destino.
- Configurei o token de autenticação gerado na aba 'Access Control' do IRIS.

<!-- Inserir imagem: wazuh_webhook_conf.png -->
<!-- Figura X - Configuração do Webhook no Wazuh para envio de alertas ao IRIS. -->

#### 2 - Geração de um alerta manual no Wazuh para teste:
``` bash
curl -vk -X POST https://<IP-do-IRIS>/alerts/add \
-H "Content-Type: application/json" \
-H "Authorization: Bearer <seu_token_IRIS>" \
-d '{
    "title": "Teste de Integração Wazuh -> IRIS",
    "description": "Este é um alerta de teste gerado manualmente via curl para validar a integração.",
    "source": "Wazuh",
    "severity": "3"
}'
```

#### 3 - Verificação na interface do IRIS:
- Confirmação de que o alerta foi recebido e criado como um novo caso.

<!-- Inserir imagem: iris_alert_received.png -->
<!-- Figura X - Visualização do alerta recebido na interface do IRIS, já registrado como novo caso para investigação. -->

## Testes de Integração: Wazuh - Shuffle (SOAR)

Para validar a comunicação entre o Wazuh e o Shuffle, configurei um fluxo básico no Shuffle para escutar por novos alertas e realizar uma ação simples de notificação.

### Etapas:

#### 1 - Configuração de um workflow de teste no Shuffle.

#### 2 - Geração de um alerta no Wazuh (similar ao teste anterior).

#### 3 - Confirmação de que o Shuffle executou a automação desejada ao receber o alerta.

Esse teste foi importante para validar a capacidade de resposta automatizada do ambiente.

<!-- as -->

## Considerações Finais sobre os Testes

O processo de testes foi essencial para garantir que todos os componentes do SOC LAB estão funcionando de forma intergada.

Além de validar as comunicações e os fluxos de alertas, esse exercício me permitiu entender melhor os pontos de integração e a importância de um bom monitoramento entre os diferentes sistemas.

Os próximos passos envolerão documentar os resultados finais e as lições aprendidas nesse projeto.
