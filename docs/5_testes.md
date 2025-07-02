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

### Resultado esperado de testes de ping:
<!-- Inserir imagem: ping_test.png -->
<!-- Img 12 - Teste de conectividade entre as VMs usando o comando ping. -->

## Testes de Comunicação entre Agentes e Wazuh Manager

Após validar a conectividade, o foco foi garantir que os agentes estavam enviando logs corretamente ao Wazuh Manager.

### Itens verificados:

- Status dos agentes no Dashboard do Wazuh;
- Geração de eventos simples para validar o pipeline de logs.

#### Exemplo de evento gerado no agente Linux:

``` bash
sudo su
```
Esse comando simples foi suficiente para gerar um evento de escalonamento de privilégio, o qual apareceu no Dashboard.

<!-- Inserir imagem: wazuh_alert_privilege_escalation.png -->
<!-- Img 13 - Alerta de escalonamento de privilégio gerado após execução do comando 'sudo su' no agente Linux -->

#### Exemplo de evento gerado no agente Windows:

- Abertura de aplicações administrativas.
- Alterações em arquivos de sistema.

Esses eventos também apareceram como alertas no Wazuh.

## Testes de Geração de Alertas Personalizados

Para validar o pipeline de detecção e resposta, realizei alguns testes manuais de geração de alertas.

#### Exemplo: Criação de arquivo suspeito em um diretório sensível

``` bash
sudo touch /tmp/teste_alerta.txt
```
<!-- Inserir imagem: wazuh_alert_file_creation.png -->
<!-- Img 14 - Alerta gerado no Wazuh após a criação de um arquivo suspeito no agente Linux. -->

Na imagem é possível ver o alerta gerado no Wazuh com base nas regras pré-configuradas.

#### Exemplo: Execução de comandos administrativos no Windows
- Execução do Prompt de Comando como administrador.
- Instalação e remoção de programas de testes.

Todos os eventos foram devidamente capturados e reportados ao Wazuh.

## Testes de Integração: Wazuh - IRIS (DFIR)

Um dos testes mais importantes foi validar a integração entre o Wazuh e o IRIS, garantindo que os alertas gerados no SIEM fossem encaminhados corretamente para o IRIS para posterior análise e investigação forense.

### Procedimento realizado:

#### 1 - Configuração de um Webhook no Wazuh:
- Inclusão do IP do IRIS como destino;
- Adição do token de autenticação no cabeçalho.

<!-- Inserir imagem: wazuh_webhook_conf.png -->
<!-- Img 15 -- Configuração do Webhook no Wazuh para envio de alertas ao IRIS. -->

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
<!-- Img 16 - Visualização do alerta recebido na interface do IRIS, já registrado como novo caso para investigação. -->

## Testes de Integração: Wazuh - Shuffle (SOAR)

Para validar a comunicação entre o Wazuh e o Shuffle, configurei um fluxo básico no Shuffle para escutar por novos alertas e realizar uma ação simples de notificação.

### Etapas:

#### 1 - Criação de um workflow de teste no Shuffle:
 - Configurado para escutar alertas via Webhook e acionar uma automação simples (ex: log de evento ou notificação).

#### 2 - Geração de evento monitorado pelo Wazuh:

- Uso de comando sudo no agente Linux;
- Criação de arquivo suspeito no /tmp.

#### 3 - Validação do workflow:

- O Shuffle recebeu corretamente o alerta JSON do Wazuh;
- O workflow foi acionado e executou conforme o esperado.

Esse teste comprovou que o ambiente possui capacidade de resposta automatizada a incidentes.

<!-- Img 17 -->

## Considerações Finais sobre os Testes

Os testes realizados confirmaram que:

- Os agentes estão se comunicando corretamente com o Wazuh;

- Alertas estão sendo gerados com base em eventos reais e personalizados;

- O SIEM está processando e roteando corretamente os alertas para o IRIS e o Shuffle;

- As integrações entre os componentes estão funcionais e seguras.

Essa etapa foi essencial para validar a arquitetura e refinar a integração entre os sistemas. Além disso, ajudou a consolidar o entendimento prático sobre os fluxos de detecção, correlação e resposta em um SOC realista.

Os próximos passos envolvem a documentação dos resultados finais e a consolidação de aprendizados técnicos no projeto.
