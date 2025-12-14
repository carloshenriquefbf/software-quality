Claude Haiku 4.5

Link da conversa: https://claude.ai/public/artifacts/d5249a25-83e0-4839-860b-6ba8f9d567dd

Prompt 1:

```
# Persona
Você é uma pessoa analista de teste de software sênior.

# Tarefa

Você recebeu a demanda de construir o plano de teste a partir do contexto fornecido abaixo, pelo documento "geral.md" que descreve o software, do documento "requisitos-biot.md" que descreve os requisitos do subsistema biot e do documento "plano-de-testes-biot.md", que descreve os planos de teste e dá o exemplo de um caso de teste detalhado usando um dos requisitos.

Você  deve utilizar o arquivo "template.md", que será fornecido, como base para  preparar o plano de testes e os casos de teste (incluindo roteiros) que apoiem o teste funcional (e, depois, não funcional) do sistema SAFE.

# Contexto

o Sistema SAFE é uma solução desenvolvida pela UFRJ no contexto da pandemia da COVID-19 com o objetivo de apoiar o uso seguro das instalações físicas da universidade. O sistema monitora continuamente indicadores de biossegurança (e.g. temperatura, umidade, nível de CO₂, VOCs e número de pessoas) em ambientes internos (salas de aula, laboratórios, auditórios etc.), seguindo as diretrizes do Guia de Biossegurança da UFRJ. A partir desses dados, o SAFE permite a avaliação do nível de risco de cada instalação, fornece informações em tempo real e emite alertas quando limites seguros são ultrapassados, apoiando ações preventivas e de mitigação de riscos.

Arquiteturalmente, o SAFE é composto por quatro subsistemas principais: o SAFE BIoT, dispositivo IoT responsável pela coleta dos dados ambientais; o Broker de Mensagens, que utiliza o protocolo MQTT para a comunicação; o SAFE Manager, núcleo do sistema que gerencia usuários, instalações, dispositivos, regras de biossegurança e armazenamento dos dados; e o SAFE Dashboard, interface visual que exibe as informações de monitoramento e estados das instalações para diferentes perfis de usuários. Embora o sistema não atue diretamente no controle do fluxo de pessoas, ele automatiza o acompanhamento das condições ambientais, o bloqueio ou liberação de espaços e a solicitação de serviços de limpeza e manutenção, contribuindo para uma gestão mais eficiente e segura dos ambientes monitorados

# Instruções

Com base no contexto do Sistema SAFE e nos requisitos funcionais e não funcionais fornecidos, elabore um plano de testes detalhado que abranja as funcionalidades descritas que forem essenciais. Certifique-se de incluir estratégias de teste, critérios de entrada e saída, ambientes de teste, e casos de teste específicos para cada requisito funcional. Utilize o template fornecido para estruturar o documento de forma clara e organizada. O formato da resposta deve ser em markdown.
```

Prompt 2:

```
Continue
```

Prompt 3:

```
essa parte não está no formato markdown:

TC_RNF11_001 – Publicação de dados de contagem de pessoas no evento

Requisito Associado: RNF11
Descrição: Verificar se o dispositivo publica dados de contagem de pessoas no tópico SAFE_ENTRY_FLOW apenas quando um evento de entrada/saída é detectado
Pré-condições:

Dispositivo BIoT ligado e operacional
Sensores HC-SR04 calibrados
Broker MQTT ativo


Passos de Teste:

Monitorar o tópico SAFE_ENTRY_FLOW durante 2 minutos sem atividade
Registrar se há publicações (não deve haver)
Simular uma entrada de pessoa
Verificar se uma mensagem é publicada imediatamente no tópico SAFE_ENTRY_FLOW
Registrar o timestamp da detecção
Simular uma saída de pessoa
Verificar se uma nova mensagem é publicada no mesmo tópico
Validar o formato JSON da mensagem


Dados de Teste:

Duração do monitoramento em repouso: 2 minutos
Número de eventos simulados: 2 (1 entrada, 1 saída)


Resultado Esperado:

Nenhuma mensagem é publicada durante o período sem atividade
Uma mensagem é publicada imediatamente após cada evento de entrada/saída
As mensagens estão no formato JSON correto com campos: idBIoT, timestamp, entry_flow, erro
O campo entry_flow reflete corretamente a natureza do evento


Prioridade: Essencial
Status: [ ] Passou | [ ] Falhou | [ ] Bloqueado

TC_RNF3_RNF12_001 – Atualização periódica de limites de biossegurança

Requisito Associado: RNF3, RNF12
Descrição: Verificar se o dispositivo BIoT atualiza os limites de biossegurança ao ligar e periodicamente a cada INTERVALO_ATUALIZACAO_LIMITES (30 minutos)
Pré-condições:

Dispositivo BIoT desligado
Broker MQTT ativo com limites publicados iniciais
Tempo de teste disponível: mínimo 35 minutos


Passos de Teste:

Energizar o dispositivo BIoT
Monitorar o tópico MQTT onde os limites são solicitados
Registrar o timestamp da primeira solicitação (deve ocorrer em < 5 segundos após ligar)
Aguardar 30 minutos + tolerância (32 minutos)
Registrar o timestamp da segunda solicitação de limites
Verificar se a diferença é aproximadamente 30 minutos
Alterar os limites no broker
Aguardar o próximo ciclo de atualização (até 32 minutos)
Verificar se o dispositivo recebeu os novos limites


Dados de Teste:

Intervalo esperado: 30 minutos
Tolerância: ±2 minutos
Número de ciclos esperados: 2 (inicial + 1 periódico)


Resultado Esperado:

O dispositivo solicita limites ao ligar (dentro de 5 segundos)
O dispositivo solicita limites periodicamente a cada 30 ± 2 minutos
Os novos limites são recebidos e aplicados dentro de um ciclo
Não há perda de solicitações ou atrasos significativos


Prioridade: Essencial
Status: [ ] Passou | [ ] Falhou | [ ] Bloqueado

TC_RNF8_001 – Sincronização de relógio com NTP

Requisito Associado: RNF8
Descrição: Verificar se o dispositivo sincroniza seu relógio com a hora da Internet sempre que publica um tópico no broker
Pré-condições:

Dispositivo BIoT ligado e conectado à rede Wi-Fi
Acesso a servidor NTP ativo
Relógio do dispositivo pode estar desincronizado inicialmente


Passos de Teste:

Definir o relógio interno do dispositivo com hora incorreta (ex: 2020-01-01 00:00:00)
Energizar o dispositivo
Aguardar a primeira publicação no broker (dentro de 15 segundos)
Capturar o timestamp da mensagem publicada
Comparar com a hora do sistema do servidor NTP
Verificar se a diferença é menor que 5 segundos
Repetir para 5 mensagens consecutivas


Dados de Teste:

Hora inicial incorreta no dispositivo
Número de mensagens analisadas: 5
Hora de referência: servidor NTP ou servidor do broker


Resultado Esperado:

Todos os timestamps nas mensagens estão sincronizados com o servidor NTP
A diferença entre o timestamp publicado e a hora do servidor é menor que 5 segundos
O dispositivo sincroniza o relógio antes de cada publicação (ou periodicamente)


Prioridade: Importante
Status: [ ] Passou | [ ] Falhou | [ ] Bloqueado


2.3 Teste de Regressão
Os testes de regressão garantem que mudanças ou correções implementadas no código não introduzem novos defeitos ou reintroduzem defeitos antigos.
2.3.1 Prazo para realização
Os testes de regressão serão executados após cada correção de bug ou implementação de nova funcionalidade. Prazo estimado: 1 semana por ciclo.
2.3.2 Recursos necessários
Hardware:

Dispositivo BIoT (pode ser o mesmo dos testes anteriores)
Broker MQTT ativo

Software:

Suite de testes automatizados (scripts em Python ou equivalente)
Cliente MQTT
Ferramentas de análise de dados

Pessoal:

1 Testador sênior para automação e análise
Suporte de desenvolvedor conforme necessário

2.3.3 Requisitos a serem testados
Todos os requisitos dos testes funcionais e de integração serão re-testados em cada ciclo de regressão.
2.3.4 Casos de Teste
Os casos de teste para regressão são os mesmos dos testes funcionais (TC_RF01_001 até TC_RF07_001) e de integração, executados de forma automatizada ou manual conforme o cronograma.
TC_REG_001 – Reexecução de todos os testes críticos

Descrição: Suite de regressão contendo todos os testes essenciais (RF01-RF04, RNF2, RNF10-12)
Executado: Após cada atualização do firmware
Critério de Sucesso: Todos os testes da suite passam sem novos defeitos


2.4 Teste Não Funcional
Os testes não funcionais validam aspectos como confiabilidade, robustez, recuperação de falhas e segurança do dispositivo BIoT.
2.4.1 Prazo para realização
Os testes não funcionais serão realizados em paralelo com os testes de integração e após, dependendo da natureza do teste. Prazo estimado: 3 semanas.
2.4.2 Recursos necessários
Hardware:

Dispositivo BIoT operacional
Broker MQTT ativo
Fonte de alimentação com capacidade de desligamento controlado
Roteador Wi-Fi com controle de conexão
Computador para monitoramento de logs

Software:

Ferramentas de desconexão/reconexão de rede (TC - Traffic Control no Linux)
Analisador de consumo de energia (se disponível)
Ferramenta de logging para capturar erros

Pessoal:

2 Testadores
1 Desenvolvedor para análise de logs

2.4.3 Requisitos a serem testados

RNF5: Precisão mínima de 90%
RNF6: Retomada automática após falta de energia
RNF7: Reconexão automática à internet
RNF9: Armazenamento local em caso de falha de conexão
RNF13: Coleta contínua sem interrupção durante semana
RNF14: Compatibilidade com NodeMCU/ESP32
RNF15: Comunicação autenticada com broker
RNF16: Segurança (sem login via Wi-Fi)
RNF17: Criptografia com protocolo Speck
RNF22: Reportagem de falhas através de etiquetas de erro

2.4.4 Casos de Teste
TC_RNF6_001 – Retomada automática após perda de energia

Requisito Associado: RNF6
Descrição: Verificar se o dispositivo BIoT retoma automaticamente suas atividades após uma interrupção no fornecimento de energia
Pré-condições:

Dispositivo BIoT ligado, operacional e publicando dados normalmente
Broker MQTT ativo
Todos os sensores funcionais
Tempo de teste: 10 minutos


Passos de Teste:

Monitorar o tópico SAFE_IAQ por 2 ciclos de publicação (30 segundos)
Registrar que o dispositivo está operacional (verde)
Desligar abruptamente a fonte de alimentação
Aguardar 10 segundos
Religar a fonte de alimentação
Monitorar o tópico SAFE_IAQ nos próximos 2 minutos
Verificar se as publicações retomam dentro de 30 segundos
Validar se os dados publicados após a retomada são válidos
Registrar se houve perda de dados ou mensagens corrompidas


Dados de Teste:

Duração do desligamento: 10 segundos
Tempo máximo esperado para retomada: 30 segundos
Número de ciclos monitorados antes: 2
Número de ciclos monitorados após: 8


Resultado Esperado:

O dispositivo se reinicia automaticamente quando a energia é restaurada
As publicações retomam dentro de 30 segundos
Nenhum dado é perdido ou corrompido
O dispositivo retorna ao estado operacional normal


Prioridade: Essencial
Status: [ ] Passou | [ ] Falhou | [ ] Bloqueado

TC_RNF7_001 – Reconexão automática à internet

Requisito Associado: RNF7
Descrição: Verificar se o dispositivo BIoT reconecta-se automaticamente à rede Wi-Fi e ao broker quando a conexão é perdida
Pré-condições:

Dispositivo BIoT ligado e operacional
Broker MQTT ativo
Roteador Wi-Fi com controle de desconexão disponível


Passos de Teste:

Confirmar que o dispositivo está conectado e publicando normalmente (2 ciclos)
Interromper a conexão Wi-Fi (desativar roteador ou isolar o dispositivo)
Monitorar o comportamento do dispositivo por 1 minuto
Restaurar a conexão Wi-Fi
Monitorar a retomada das publicações
Registrar o tempo decorrido para reconexão (desde o restabelecimento até a primeira publicação)
Validar se os dados publicados após reconexão são válidos


Dados de Teste:

Duração da desconexão: 1 minuto
Tempo máximo esperado para reconexão e primeira publicação: 60 segundos


Resultado Esperado:

O dispositivo detecta a perda de conexão
Tenta se reconectar automaticamente quando a rede volta
Retoma as publicações dentro de 60 segundos
Nenhum dado é perdido durante o processo de reconexão


Prioridade: Essencial
Status: [ ] Passou | [ ] Falhou | [ ] Bloqueado

TC_RNF9_001 – Armazenamento local em caso de falha de conexão

Requisito Associado: RNF9
Descrição: Verificar se o dispositivo BIoT armazena dados localmente quando há falha de conexão e os envia ao broker quando a conexão é restabelecida
Pré-condições:

Dispositivo BIoT ligado e operacional
Broker MQTT ativo
Memória local do dispositivo disponível
Capacidade de interromper a conexão com o broker


Passos de Teste:

Confirmar que o dispositivo está publicando normalmente (1 ciclo)
Interromper a conexão com o broker (manter Wi-Fi ativo)
Aguardar 3 ciclos de coleta de dados (45 segundos) sem conexão com o broker
Registrar que durante este período, nenhuma publicação ocorre no broker
Restaurar a conexão com o broker
Monitorar o tópico SAFE_IAQ nos próximos 2 minutos
Verificar se os dados que foram coletados durante a desconexão são publicados
Validar se os dados enviados têm timestamps corretos (relativos ao momento da coleta)


Dados de Teste:

Duração da desconexão do broker: 45 segundos (3 ciclos)
Número esperado de ciclos armazenados: 3


Resultado Esperado:

O dispositivo coleta dados mesmo sem conexão com o broker
Os dados são armazenados localmente
Quando a conexão é restabelecida, todos os dados armazenados são enviados
Os timestamps dos dados refletem o momento original da coleta
Nenhum dado é perdido durante o processo


Prioridade: Essencial
Status: [ ] Passou | [ ] Falhou | [ ] Bloqueado

TC_RNF13_001 – Coleta contínua durante a semana

Requisito Associado: RNF13
Descrição: Verificar se o dispositivo BIoT coleta e envia informações continuamente durante a semana, seguindo os intervalos especificados
Pré-condições:

Dispositivo BIoT ligado continuamente por 7 dias
Broker MQTT ativo durante todo o período
Todos os sensores funcionais
Monitoramento contínuo dos tópicos MQTT


Passos de Teste:

Energizar o dispositivo e iniciá-lo no domingo
Monitorar os tópicos SAFE_IAQ e SAFE_ENTRY_FLOW continuamente
Registrar o número de publicações esperadas por dia: 5760 mensagens SAFE_IAQ (1 a cada 15 seg × 86400 seg/dia)
Registrar o número real de mensagens recebidas
Calcular a taxa de sucesso: (mensagens_reais / mensagens_esperadas) × 100
Registrar qualquer interrupção ou anomalia
Avaliar a taxa de sucesso no final de 7 dias


Dados de Teste:

Duração do teste: 7 dias
Intervalo de publicação: 15 segundos para SAFE_IAQ
Mensagens esperadas: ~5760 por dia


Resultado Esperado:

O dispositivo publica continuamente durante os 7 dias
A taxa de sucesso é superior a 99% (perda máxima aceitável: ~57 mensagens em 7 dias)
Não há interrupções não planejadas
Todos os dados publicados são válidos


Prioridade: Essencial
Status: [ ] Passou | [ ] Falhou | [ ] Bloqueado

TC_RNF22_001 – Reportagem de falhas através de etiquetas de erro

Requisito Associado: RNF22
Descrição: Verificar se o dispositivo BIoT reporta corretamente falhas de coleta de medidas através de etiquetas de erro no campo "erro" das mensagens MQTT
Pré-condições:

Dispositivo BIoT ligado
Broker MQTT ativo
Capacidade de desativar sensores ou simular falhas


Passos de Teste:

Confirmar operação normal do dispositivo (erro = 0)
Desativar ou desconectar o sensor DHT11/AHT21 (sensor de temperatura/umidade)
Monitorar a próxima publicação de SAFE_IAQ
Verificar se o campo "erro" contém a flag ERROR_TEMP (2) e ERROR_HUMID (4)
Validar se o valor de erro é 6 (2 + 4 em binário)
Reconectar o sensor e monitorar se o erro volta a 0
Repetir para o sensor CJMCU-811/ENS160 (verificar flag ERROR_CO2 = 8 ou ERROR_TVOC = 16)
Testar múltiplas falhas simultâneas (ex: temperatura e CO2)
Validar se os erros se combinam corretamente em binário


Dados de Teste:

Sensores a desativar: DHT11, CJMCU-811
Flags esperadas: ERROR_TEMP (2), ERROR_HUMID (4), ERROR_CO2 (8), ERROR_TVOC (16)
Combinações: erro único, erro duplo, erro triplo


Resultado Esperado:

Cada falha é corretamente reportada com a flag correspondente
Múltiplas falhas são reportadas como a soma binária das flags
O campo "erro" é 0 quando não há falhas
As flags são mantidas enquanto a falha persiste
As flags são limpas quando a falha é resolvida


Prioridade: Importante
Status: [ ] Passou | [ ] Falhou | [ ] Bloqueado


3. Resultados dos Testes
Esta seção será preenchida conforme os testes forem executados. Para cada tipo de teste e caso de teste, será documentado:

Data de execução
Resultado (Passou/Falhou/Bloqueado)
Observações e problemas encontrados
Ações corretivas tomadas
Data da resolução

Resumo de Testes Funcionais
ID do CasoDescriçãoStatusDataObservaçõesTC_RF01_001Obtenção de limites na inicialização[ ]TC_RF02_001Contabilização de entrada[ ]TC_RF02_002Contabilização de saída[ ]TC_RF03_001Coleta de temperatura[ ]TC_RF04_001Coleta de CO₂[ ]TC_RF05_001Apresentação do estado[ ]TC_RF06_001Coleta de umidade[ ]TC_RF07_001Coleta de VOCs[ ]
Resumo de Testes de Integração
ID do CasoDescriçãoStatusDataObservaçõesTC_RNF2_001Formato JSON SAFE_IAQ[ ]TC_RNF10_001Intervalo de publicação[ ]TC_RNF11_001Publicação em evento[ ]TC_RNF3_RNF12_001Atualização de limites[ ]TC_RNF8_001Sincronização NTP[ ]
Resumo de Testes Não Funcionais
ID do CasoDescriçãoStatusDataObservaçõesTC_RNF6_001Retomada após falha de energia[ ]TC_RNF7_001Reconexão automática[ ]TC_RNF9_001Armazenamento local[ ]TC_RNF13_001Coleta contínua semanal[ ]TC_RNF22_001Reportagem de erros[ ]

Critérios de Entrada

 Todos os requisitos funcionais estão finalizados e revisados
 Ambiente de teste configurado (broker MQTT, sensores calibrados)
 Versão estável do firmware pronta para testes
 Documentação técnica completa disponível
 Equipe de testes alocada e treinada

Critérios de Saída

 Todos os casos de teste essenciais (RF01-RF04, RNF1-RNF3, RNF6-RNF12) aprovados
 Cobertura de teste mínima de 90% para requisitos essenciais
 Taxa de sucesso em testes de longa duração (RNF13) ≥ 99%
 Número aceitável de defeitos abertos (máximo 3 críticos, máximo 5 maiores)
 Concordância da equipe sobre a qualidade do produto
 Todos os defeitos críticos resolvidos
 Testes de regressão passaram na versão final</parameter>
 ```

 Prompt 4:

```
crie um novo artefato no formato markdown com essa parte do planejamento de testes:

TC_RNF11_001 – Publicação de dados de contagem de pessoas no evento

Requisito Associado: RNF11
Descrição: Verificar se o dispositivo publica dados de contagem de pessoas no tópico SAFE_ENTRY_FLOW apenas quando um evento de entrada/saída é detectado
Pré-condições:

Dispositivo BIoT ligado e operacional
Sensores HC-SR04 calibrados
Broker MQTT ativo


Passos de Teste:

Monitorar o tópico SAFE_ENTRY_FLOW durante 2 minutos sem atividade
Registrar se há publicações (não deve haver)
Simular uma entrada de pessoa
Verificar se uma mensagem é publicada imediatamente no tópico SAFE_ENTRY_FLOW
Registrar o timestamp da detecção
Simular uma saída de pessoa
Verificar se uma nova mensagem é publicada no mesmo tópico
Validar o formato JSON da mensagem


Dados de Teste:

Duração do monitoramento em repouso: 2 minutos
Número de eventos simulados: 2 (1 entrada, 1 saída)


Resultado Esperado:

Nenhuma mensagem é publicada durante o período sem atividade
Uma mensagem é publicada imediatamente após cada evento de entrada/saída
As mensagens estão no formato JSON correto com campos: idBIoT, timestamp, entry_flow, erro
O campo entry_flow reflete corretamente a natureza do evento


Prioridade: Essencial
Status: [ ] Passou | [ ] Falhou | [ ] Bloqueado

TC_RNF3_RNF12_001 – Atualização periódica de limites de biossegurança

Requisito Associado: RNF3, RNF12
Descrição: Verificar se o dispositivo BIoT atualiza os limites de biossegurança ao ligar e periodicamente a cada INTERVALO_ATUALIZACAO_LIMITES (30 minutos)
Pré-condições:

Dispositivo BIoT desligado
Broker MQTT ativo com limites publicados iniciais
Tempo de teste disponível: mínimo 35 minutos


Passos de Teste:

Energizar o dispositivo BIoT
Monitorar o tópico MQTT onde os limites são solicitados
Registrar o timestamp da primeira solicitação (deve ocorrer em < 5 segundos após ligar)
Aguardar 30 minutos + tolerância (32 minutos)
Registrar o timestamp da segunda solicitação de limites
Verificar se a diferença é aproximadamente 30 minutos
Alterar os limites no broker
Aguardar o próximo ciclo de atualização (até 32 minutos)
Verificar se o dispositivo recebeu os novos limites


Dados de Teste:

Intervalo esperado: 30 minutos
Tolerância: ±2 minutos
Número de ciclos esperados: 2 (inicial + 1 periódico)


Resultado Esperado:

O dispositivo solicita limites ao ligar (dentro de 5 segundos)
O dispositivo solicita limites periodicamente a cada 30 ± 2 minutos
Os novos limites são recebidos e aplicados dentro de um ciclo
Não há perda de solicitações ou atrasos significativos


Prioridade: Essencial
Status: [ ] Passou | [ ] Falhou | [ ] Bloqueado

TC_RNF8_001 – Sincronização de relógio com NTP

Requisito Associado: RNF8
Descrição: Verificar se o dispositivo sincroniza seu relógio com a hora da Internet sempre que publica um tópico no broker
Pré-condições:

Dispositivo BIoT ligado e conectado à rede Wi-Fi
Acesso a servidor NTP ativo
Relógio do dispositivo pode estar desincronizado inicialmente


Passos de Teste:

Definir o relógio interno do dispositivo com hora incorreta (ex: 2020-01-01 00:00:00)
Energizar o dispositivo
Aguardar a primeira publicação no broker (dentro de 15 segundos)
Capturar o timestamp da mensagem publicada
Comparar com a hora do sistema do servidor NTP
Verificar se a diferença é menor que 5 segundos
Repetir para 5 mensagens consecutivas


Dados de Teste:

Hora inicial incorreta no dispositivo
Número de mensagens analisadas: 5
Hora de referência: servidor NTP ou servidor do broker


Resultado Esperado:

Todos os timestamps nas mensagens estão sincronizados com o servidor NTP
A diferença entre o timestamp publicado e a hora do servidor é menor que 5 segundos
O dispositivo sincroniza o relógio antes de cada publicação (ou periodicamente)


Prioridade: Importante
Status: [ ] Passou | [ ] Falhou | [ ] Bloqueado


2.3 Teste de Regressão
Os testes de regressão garantem que mudanças ou correções implementadas no código não introduzem novos defeitos ou reintroduzem defeitos antigos.
2.3.1 Prazo para realização
Os testes de regressão serão executados após cada correção de bug ou implementação de nova funcionalidade. Prazo estimado: 1 semana por ciclo.
2.3.2 Recursos necessários
Hardware:

Dispositivo BIoT (pode ser o mesmo dos testes anteriores)
Broker MQTT ativo

Software:

Suite de testes automatizados (scripts em Python ou equivalente)
Cliente MQTT
Ferramentas de análise de dados

Pessoal:

1 Testador sênior para automação e análise
Suporte de desenvolvedor conforme necessário

2.3.3 Requisitos a serem testados
Todos os requisitos dos testes funcionais e de integração serão re-testados em cada ciclo de regressão.
2.3.4 Casos de Teste
Os casos de teste para regressão são os mesmos dos testes funcionais (TC_RF01_001 até TC_RF07_001) e de integração, executados de forma automatizada ou manual conforme o cronograma.
TC_REG_001 – Reexecução de todos os testes críticos

Descrição: Suite de regressão contendo todos os testes essenciais (RF01-RF04, RNF2, RNF10-12)
Executado: Após cada atualização do firmware
Critério de Sucesso: Todos os testes da suite passam sem novos defeitos


2.4 Teste Não Funcional
Os testes não funcionais validam aspectos como confiabilidade, robustez, recuperação de falhas e segurança do dispositivo BIoT.
2.4.1 Prazo para realização
Os testes não funcionais serão realizados em paralelo com os testes de integração e após, dependendo da natureza do teste. Prazo estimado: 3 semanas.
2.4.2 Recursos necessários
Hardware:

Dispositivo BIoT operacional
Broker MQTT ativo
Fonte de alimentação com capacidade de desligamento controlado
Roteador Wi-Fi com controle de conexão
Computador para monitoramento de logs

Software:

Ferramentas de desconexão/reconexão de rede (TC - Traffic Control no Linux)
Analisador de consumo de energia (se disponível)
Ferramenta de logging para capturar erros

Pessoal:

2 Testadores
1 Desenvolvedor para análise de logs

2.4.3 Requisitos a serem testados

RNF5: Precisão mínima de 90%
RNF6: Retomada automática após falta de energia
RNF7: Reconexão automática à internet
RNF9: Armazenamento local em caso de falha de conexão
RNF13: Coleta contínua sem interrupção durante semana
RNF14: Compatibilidade com NodeMCU/ESP32
RNF15: Comunicação autenticada com broker
RNF16: Segurança (sem login via Wi-Fi)
RNF17: Criptografia com protocolo Speck
RNF22: Reportagem de falhas através de etiquetas de erro

2.4.4 Casos de Teste
TC_RNF6_001 – Retomada automática após perda de energia

Requisito Associado: RNF6
Descrição: Verificar se o dispositivo BIoT retoma automaticamente suas atividades após uma interrupção no fornecimento de energia
Pré-condições:

Dispositivo BIoT ligado, operacional e publicando dados normalmente
Broker MQTT ativo
Todos os sensores funcionais
Tempo de teste: 10 minutos


Passos de Teste:

Monitorar o tópico SAFE_IAQ por 2 ciclos de publicação (30 segundos)
Registrar que o dispositivo está operacional (verde)
Desligar abruptamente a fonte de alimentação
Aguardar 10 segundos
Religar a fonte de alimentação
Monitorar o tópico SAFE_IAQ nos próximos 2 minutos
Verificar se as publicações retomam dentro de 30 segundos
Validar se os dados publicados após a retomada são válidos
Registrar se houve perda de dados ou mensagens corrompidas


Dados de Teste:

Duração do desligamento: 10 segundos
Tempo máximo esperado para retomada: 30 segundos
Número de ciclos monitorados antes: 2
Número de ciclos monitorados após: 8


Resultado Esperado:

O dispositivo se reinicia automaticamente quando a energia é restaurada
As publicações retomam dentro de 30 segundos
Nenhum dado é perdido ou corrompido
O dispositivo retorna ao estado operacional normal


Prioridade: Essencial
Status: [ ] Passou | [ ] Falhou | [ ] Bloqueado

TC_RNF7_001 – Reconexão automática à internet

Requisito Associado: RNF7
Descrição: Verificar se o dispositivo BIoT reconecta-se automaticamente à rede Wi-Fi e ao broker quando a conexão é perdida
Pré-condições:

Dispositivo BIoT ligado e operacional
Broker MQTT ativo
Roteador Wi-Fi com controle de desconexão disponível


Passos de Teste:

Confirmar que o dispositivo está conectado e publicando normalmente (2 ciclos)
Interromper a conexão Wi-Fi (desativar roteador ou isolar o dispositivo)
Monitorar o comportamento do dispositivo por 1 minuto
Restaurar a conexão Wi-Fi
Monitorar a retomada das publicações
Registrar o tempo decorrido para reconexão (desde o restabelecimento até a primeira publicação)
Validar se os dados publicados após reconexão são válidos


Dados de Teste:

Duração da desconexão: 1 minuto
Tempo máximo esperado para reconexão e primeira publicação: 60 segundos


Resultado Esperado:

O dispositivo detecta a perda de conexão
Tenta se reconectar automaticamente quando a rede volta
Retoma as publicações dentro de 60 segundos
Nenhum dado é perdido durante o processo de reconexão


Prioridade: Essencial
Status: [ ] Passou | [ ] Falhou | [ ] Bloqueado

TC_RNF9_001 – Armazenamento local em caso de falha de conexão

Requisito Associado: RNF9
Descrição: Verificar se o dispositivo BIoT armazena dados localmente quando há falha de conexão e os envia ao broker quando a conexão é restabelecida
Pré-condições:

Dispositivo BIoT ligado e operacional
Broker MQTT ativo
Memória local do dispositivo disponível
Capacidade de interromper a conexão com o broker


Passos de Teste:

Confirmar que o dispositivo está publicando normalmente (1 ciclo)
Interromper a conexão com o broker (manter Wi-Fi ativo)
Aguardar 3 ciclos de coleta de dados (45 segundos) sem conexão com o broker
Registrar que durante este período, nenhuma publicação ocorre no broker
Restaurar a conexão com o broker
Monitorar o tópico SAFE_IAQ nos próximos 2 minutos
Verificar se os dados que foram coletados durante a desconexão são publicados
Validar se os dados enviados têm timestamps corretos (relativos ao momento da coleta)


Dados de Teste:

Duração da desconexão do broker: 45 segundos (3 ciclos)
Número esperado de ciclos armazenados: 3


Resultado Esperado:

O dispositivo coleta dados mesmo sem conexão com o broker
Os dados são armazenados localmente
Quando a conexão é restabelecida, todos os dados armazenados são enviados
Os timestamps dos dados refletem o momento original da coleta
Nenhum dado é perdido durante o processo


Prioridade: Essencial
Status: [ ] Passou | [ ] Falhou | [ ] Bloqueado

TC_RNF13_001 – Coleta contínua durante a semana

Requisito Associado: RNF13
Descrição: Verificar se o dispositivo BIoT coleta e envia informações continuamente durante a semana, seguindo os intervalos especificados
Pré-condições:

Dispositivo BIoT ligado continuamente por 7 dias
Broker MQTT ativo durante todo o período
Todos os sensores funcionais
Monitoramento contínuo dos tópicos MQTT


Passos de Teste:

Energizar o dispositivo e iniciá-lo no domingo
Monitorar os tópicos SAFE_IAQ e SAFE_ENTRY_FLOW continuamente
Registrar o número de publicações esperadas por dia: 5760 mensagens SAFE_IAQ (1 a cada 15 seg × 86400 seg/dia)
Registrar o número real de mensagens recebidas
Calcular a taxa de sucesso: (mensagens_reais / mensagens_esperadas) × 100
Registrar qualquer interrupção ou anomalia
Avaliar a taxa de sucesso no final de 7 dias


Dados de Teste:

Duração do teste: 7 dias
Intervalo de publicação: 15 segundos para SAFE_IAQ
Mensagens esperadas: ~5760 por dia


Resultado Esperado:

O dispositivo publica continuamente durante os 7 dias
A taxa de sucesso é superior a 99% (perda máxima aceitável: ~57 mensagens em 7 dias)
Não há interrupções não planejadas
Todos os dados publicados são válidos


Prioridade: Essencial
Status: [ ] Passou | [ ] Falhou | [ ] Bloqueado

TC_RNF22_001 – Reportagem de falhas através de etiquetas de erro

Requisito Associado: RNF22
Descrição: Verificar se o dispositivo BIoT reporta corretamente falhas de coleta de medidas através de etiquetas de erro no campo "erro" das mensagens MQTT
Pré-condições:

Dispositivo BIoT ligado
Broker MQTT ativo
Capacidade de desativar sensores ou simular falhas


Passos de Teste:

Confirmar operação normal do dispositivo (erro = 0)
Desativar ou desconectar o sensor DHT11/AHT21 (sensor de temperatura/umidade)
Monitorar a próxima publicação de SAFE_IAQ
Verificar se o campo "erro" contém a flag ERROR_TEMP (2) e ERROR_HUMID (4)
Validar se o valor de erro é 6 (2 + 4 em binário)
Reconectar o sensor e monitorar se o erro volta a 0
Repetir para o sensor CJMCU-811/ENS160 (verificar flag ERROR_CO2 = 8 ou ERROR_TVOC = 16)
Testar múltiplas falhas simultâneas (ex: temperatura e CO2)
Validar se os erros se combinam corretamente em binário


Dados de Teste:

Sensores a desativar: DHT11, CJMCU-811
Flags esperadas: ERROR_TEMP (2), ERROR_HUMID (4), ERROR_CO2 (8), ERROR_TVOC (16)
Combinações: erro único, erro duplo, erro triplo


Resultado Esperado:

Cada falha é corretamente reportada com a flag correspondente
Múltiplas falhas são reportadas como a soma binária das flags
O campo "erro" é 0 quando não há falhas
As flags são mantidas enquanto a falha persiste
As flags são limpas quando a falha é resolvida


Prioridade: Importante
Status: [ ] Passou | [ ] Falhou | [ ] Bloqueado


3. Resultados dos Testes
Esta seção será preenchida conforme os testes forem executados. Para cada tipo de teste e caso de teste, será documentado:

Data de execução
Resultado (Passou/Falhou/Bloqueado)
Observações e problemas encontrados
Ações corretivas tomadas
Data da resolução

Resumo de Testes Funcionais
ID do CasoDescriçãoStatusDataObservaçõesTC_RF01_001Obtenção de limites na inicialização[ ]TC_RF02_001Contabilização de entrada[ ]TC_RF02_002Contabilização de saída[ ]TC_RF03_001Coleta de temperatura[ ]TC_RF04_001Coleta de CO₂[ ]TC_RF05_001Apresentação do estado[ ]TC_RF06_001Coleta de umidade[ ]TC_RF07_001Coleta de VOCs[ ]
Resumo de Testes de Integração
ID do CasoDescriçãoStatusDataObservaçõesTC_RNF2_001Formato JSON SAFE_IAQ[ ]TC_RNF10_001Intervalo de publicação[ ]TC_RNF11_001Publicação em evento[ ]TC_RNF3_RNF12_001Atualização de limites[ ]TC_RNF8_001Sincronização NTP[ ]
Resumo de Testes Não Funcionais
ID do CasoDescriçãoStatusDataObservaçõesTC_RNF6_001Retomada após falha de energia[ ]TC_RNF7_001Reconexão automática[ ]TC_RNF9_001Armazenamento local[ ]TC_RNF13_001Coleta contínua semanal[ ]TC_RNF22_001Reportagem de erros[ ]

Critérios de Entrada

 Todos os requisitos funcionais estão finalizados e revisados
 Ambiente de teste configurado (broker MQTT, sensores calibrados)
 Versão estável do firmware pronta para testes
 Documentação técnica completa disponível
 Equipe de testes alocada e treinada

Critérios de Saída

 Todos os casos de teste essenciais (RF01-RF04, RNF1-RNF3, RNF6-RNF12) aprovados
 Cobertura de teste mínima de 90% para requisitos essenciais
 Taxa de sucesso em testes de longa duração (RNF13) ≥ 99%
 Número aceitável de defeitos abertos (máximo 3 críticos, máximo 5 maiores)
 Concordância da equipe sobre a qualidade do produto
 Todos os defeitos críticos resolvidos
 Testes de regressão passaram na versão final</parameter>
 ```