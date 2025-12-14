# Plano de Testes do Sistema SAFE BIoT — versão 1.0.0

---

## Histórico de Revisão

| **Data** | **Versão** | **Descrição** | **Autor** |
|----------|------------|---------------|-----------|
| 14/12/2025 | 1.0.0 | Criação inicial do plano de testes para o subsistema BIoT | Analista de Testes Sênior |

---

## Sumário

1. [Introdução](#1-introdução)
   1.1. [Finalidade](#11-finalidade)
   1.2. [Escopo](#12-escopo)
   1.3. [Definições, Acrônimos, e Abreviações](#13-definições-acrônimos-e-abreviações)
   1.4. [Referências](#14-referências)
   1.5. [Visão geral](#15-visão-geral)
2. [Estratégia de Teste](#2-estratégia-de-teste)
   2.1. [Teste Funcional](#21-teste-funcional)
   2.2. [Teste de Integração](#22-teste-de-integração)
   2.3. [Teste de Regressão](#23-teste-de-regressão)
   2.4. [Teste Não Funcional](#24-teste-não-funcional)
3. [Resultados dos Testes](#3-resultados-dos-testes)

---

# 1. Introdução

### 1.1 Finalidade

Este documento descreve o plano de testes para o subsistema SAFE BIoT, dispositivo IoT responsável pela coleta contínua de dados ambientais (temperatura, umidade, CO₂, VOCs e número de pessoas) em instalações da UFRJ. O objetivo principal é garantir que todas as funcionalidades do BIoT sejam implementadas conforme os requisitos especificados, apresentando estabilidade, confiabilidade, segurança e integridade de dados adequadas.

### 1.2 Escopo

O escopo deste plano abrange todos os requisitos funcionais (RF01 a RF07) e requisitos não funcionais (RNF1 a RNF22) do subsistema SAFE BIoT, incluindo:

- Obtenção de limites de biossegurança do broker MQTT
- Coleta de dados dos sensores (temperatura, umidade, CO₂, VOCs, contagem de pessoas)
- Comunicação com o broker via protocolo MQTT
- Formatação e envio de dados em JSON
- Tratamento de falhas e erros
- Armazenamento local de dados em caso de desconexão
- Sincronização de relógio com servidores NTP
- Segurança da comunicação e autenticação
- Funcionamento contínuo em operação normal

### 1.3 Definições, Acrônimos, e Abreviações

| Termo | Descrição |
|-------|-----------|
| **BIoT** | Dispositivo IoT para coleta de dados ambientais e biossegurança |
| **MQTT** | Message Queuing Telemetry Transport — protocolo de comunicação leve |
| **JSON** | JavaScript Object Notation — formato de intercâmbio de dados |
| **DHT11** | Sensor de temperatura e umidade |
| **AHT21** | Sensor de temperatura e umidade alternativo |
| **HC-SR04** | Sensor ultrassônico para detecção de movimento/contagem de pessoas |
| **CJMCU-811** | Sensor de CO₂ e VOCs |
| **ENS160** | Sensor de CO₂ e VOCs alternativo |
| **ESP32** | Microcontrolador com suporte a FreeRTOS e dual-core |
| **RTC** | Real Time Clock — relógio em tempo real |
| **NTP** | Network Time Protocol — protocolo de sincronização de hora |
| **VOCs** | Compostos Orgânicos Voláteis — medidos em ppb (partes por bilhão) |
| **CO₂** | Dióxido de carbono — medido em ppm (partes por milhão) |
| **eCO₂** | CO₂ equivalente |
| **RF** | Requisito Funcional |
| **RNF** | Requisito Não Funcional |
| **TC** | Test Case — Caso de Teste |

### 1.4 Referências

| **Título** | **Versão** | **Data** | **Onde pode ser obtido** |
|------------|------------|----------|--------------------------|
| Requisitos BIoT | 1.0 | 14/12/2025 | requisitos-biot.md |
| Documento Geral do Sistema SAFE | 1.0 | 14/12/2025 | geral.md |
| Template de Plano de Testes | 1.0.0 | 14/12/2025 | template.md |

### 1.5 Visão geral

Este plano de testes está organizado em duas seções principais: a Estratégia de Teste, que descreve os tipos de teste a serem realizados, seus prazos, recursos necessários, requisitos associados e casos de teste; e Resultados dos Testes, que será preenchida conforme os testes forem executados. O documento segue a estrutura do template fornecido, adaptada ao contexto específico do subsistema BIoT do sistema SAFE.

---

# 2. Estratégia de Teste

## 2.1 Teste Funcional

Os testes funcionais verificam se cada função do dispositivo BIoT opera conforme especificado nos requisitos funcionais e não funcionais, validando o comportamento esperado em diferentes cenários de operação.

### 2.1.1 Prazo para realização

Os testes funcionais serão realizados em duas fases:
- **Fase 1 (Testes Unitários):** Após a implementação de cada módulo sensor e antes da integração com o broker. Prazo estimado: 2 semanas.
- **Fase 2 (Testes de Aceitação):** Após a integração completa do dispositivo com o broker e antes do deploy em campo. Prazo estimado: 3 semanas.

### 2.1.2 Recursos necessários

**Hardware:**
- Dispositivo BIoT com ESP32 configurado
- Sensores: DHT11 ou AHT21, HC-SR04 (2 unidades mínimo), CJMCU-811 ou ENS160
- Fonte de alimentação 5V com porta micro USB
- Computador para monitoramento e captura de logs
- Broker MQTT (RabbitMQ) ativo e acessível
- Calibradores e equipamentos de teste para validação de sensores
- Câmaras climáticas ou dispositivos para variar temperatura e umidade (quando possível)
- Simuladores de entrada/saída de pessoas

**Software:**
- Ambiente de desenvolvimento (Arduino IDE ou PlatformIO)
- Cliente MQTT para monitoramento (mosquitto_sub, MQTT.fx ou similar)
- Ferramentas de captura de tráfego de rede (Wireshark)
- Logger de dados para armazenamento e análise de resultados

**Pessoal:**
- 2 Testadores (um sênior e um júnior)
- 1 Desenvolvedor para suporte durante testes
- 1 Engenheiro de Infraestrutura para gerenciar o broker MQTT

### 2.1.3 Requisitos a serem testados

**Requisitos Funcionais:**
- RF01: Obtenção dos limites de temperatura, CO₂ e ocupação
- RF02: Contabilização de movimentos de entrada e saída
- RF03: Coleta de temperatura
- RF04: Coleta de CO₂ (eCO₂)
- RF05: Apresentação do estado da instalação e sinal de risco
- RF06: Coleta de umidade
- RF07: Coleta de VOCs

**Requisitos Não Funcionais (relacionados a funcionalidade):**
- RNF2: Envio de dados em formato JSON
- RNF4: Consulta de parâmetros no broker
- RNF5: Precisão mínima de 90% nas medições
- RNF10: Publicação de dados de qualidade do ar a cada 15 segundos
- RNF11: Publicação de dados de contagem de pessoas quando detectado evento
- RNF12: Atualização de limites ao ligar e a cada INTERVALO_ATUALIZACAO_LIMITES
- RNF22: Reportagem de falhas através de etiquetas de erro

### 2.1.4 Casos de Teste

#### **TC_RF01_001 – Obtenção dos limites de temperatura, CO₂ e ocupação na inicialização**

- **Requisito Associado:** RF01
- **Descrição:** Verificar se o dispositivo BIoT obtém corretamente os limites de temperatura, CO₂ e ocupação do broker no momento da inicialização
- **Pré-condições:**
  - Broker MQTT ativo com os parâmetros publicados no tópico configurado
  - Dispositivo BIoT desligado
  - Credenciais de acesso ao broker configuradas no dispositivo
- **Passos de Teste:**
  1. Energizar o dispositivo BIoT
  2. Monitorar a comunicação MQTT entre o dispositivo e o broker usando mosquitto_sub ou ferramenta equivalente
  3. Capturar o primeiro tópico subscrito referente aos parâmetros de biossegurança
  4. Verificar se a mensagem JSON recebida contém: idBIoT, temperaturaMin, temperaturaMax, umidadeMin, umidadeMax, numeroPessoasMin, numeroPessoasMax
  5. Validar se os valores recebidos correspondem aos valores publicados no broker
  6. Registrar o timestamp de recebimento
- **Dados de Teste:**
  - Limites publicados no broker: temperaturaMin=15°C, temperaturaMax=28°C, umidadeMin=30%, umidadeMax=60%, numeroPessoasMin=0, numeroPessoasMax=40
- **Resultado Esperado:**
  - O dispositivo BIoT conecta-se ao broker com sucesso na inicialização
  - Recebe a mensagem JSON com os limites de biossegurança dentro de 5 segundos após a inicialização
  - Os valores recebidos correspondem exatamente aos valores publicados no broker
  - Não há erros na conexão ou na decodificação da mensagem JSON
- **Prioridade:** Essencial
- **Status:** [ ] Passou | [ ] Falhou | [ ] Bloqueado

#### **TC_RF02_001 – Contabilização de entrada de pessoas**

- **Requisito Associado:** RF02
- **Descrição:** Verificar se o dispositivo BIoT contabiliza corretamente a entrada de uma pessoa utilizando os sensores HC-SR04
- **Pré-condições:**
  - Dispositivo BIoT ligado e operacional
  - Sensores HC-SR04 calibrados e testados individualmente
  - Dois sensores HC-SR04 posicionados adequadamente na porta (entrada e saída)
  - Broker MQTT ativo
  - Nenhuma pessoa presente no ambiente antes do teste
- **Passos de Teste:**
  1. Monitorar o tópico MQTT SAFE_ENTRY_FLOW
  2. Registrar o contador inicial de pessoas
  3. Uma pessoa simula uma entrada passando pelos sensores HC-SR04
  4. Aguardar detecção e envio da mensagem MQTT
  5. Verificar a mensagem JSON recebida: idBIoT, timestamp, entry_flow, erro
  6. Validar se entry_flow foi incrementado em 1
  7. Registrar o timestamp da detecção
  8. Repetir o passo 3-7 mais 4 vezes (total de 5 entradas)
- **Dados de Teste:**
  - Número de simulações de entrada: 5
  - Intervalo entre entradas: 10 segundos
- **Resultado Esperado:**
  - Cada entrada é corretamente detectada pelos sensores HC-SR04
  - O campo entry_flow é incrementado em 1 para cada entrada detectada
  - As mensagens JSON são válidas e publicadas no tópico SAFE_ENTRY_FLOW
  - O timestamp está sincronizado com a hora do servidor
  - Nenhuma entrada é perdida ou duplicada
- **Prioridade:** Essencial
- **Status:** [ ] Passou | [ ] Falhou | [ ] Bloqueado

#### **TC_RF02_002 – Contabilização de saída de pessoas**

- **Requisito Associado:** RF02
- **Descrição:** Verificar se o dispositivo BIoT contabiliza corretamente a saída de uma pessoa utilizando os sensores HC-SR04
- **Pré-condições:**
  - Dispositivo BIoT ligado e operacional
  - Sensores HC-SR04 calibrados
  - Contador de pessoas no ambiente é 5 (resultado do teste TC_RF02_001 ou pré-carregado)
  - Broker MQTT ativo
- **Passos de Teste:**
  1. Monitorar o tópico MQTT SAFE_ENTRY_FLOW
  2. Registrar o contador inicial de pessoas (esperado: 5)
  3. Uma pessoa simula uma saída passando pelos sensores em ordem reversa
  4. Aguardar detecção e envio da mensagem MQTT
  5. Verificar a mensagem JSON: idBIoT, timestamp, entry_flow, erro
  6. Validar se entry_flow foi decrementado em 1 (agora igual a 4)
  7. Registrar o timestamp da detecção
  8. Repetir passos 3-7 mais 4 vezes
- **Dados de Teste:**
  - Número de simulações de saída: 5
  - Intervalo entre saídas: 10 segundos
- **Resultado Esperado:**
  - Cada saída é corretamente detectada pelos sensores
  - O campo entry_flow é decrementado em 1 para cada saída
  - As mensagens JSON são válidas e publicadas no tópico SAFE_ENTRY_FLOW
  - O contador final deve ser 0 (5 entradas - 5 saídas)
  - Nenhuma saída é perdida ou duplicada
- **Prioridade:** Essencial
- **Status:** [ ] Passou | [ ] Falhou | [ ] Bloqueado

#### **TC_RF03_001 – Coleta de temperatura**

- **Requisito Associado:** RF03
- **Descrição:** Verificar se o dispositivo BIoT coleta corretamente a temperatura da instalação utilizando o sensor DHT11 ou AHT21
- **Pré-condições:**
  - Dispositivo BIoT ligado e operacional
  - Sensor DHT11 ou AHT21 calibrado e testado individualmente
  - Ambiente com temperatura estável
  - Broker MQTT ativo
  - Termômetro de referência para validação disponível
- **Passos de Teste:**
  1. Registrar a temperatura do ambiente usando um termômetro de referência (temperatura real)
  2. Monitorar o tópico MQTT SAFE_IAQ por 5 ciclos consecutivos (15 segundos × 5 = 75 segundos)
  3. Para cada ciclo, capturar a mensagem JSON: idBIoT, timestamp, temperatura, umidade, co2, vocs, erro
  4. Extrair o valor do campo "temperatura"
  5. Comparar o valor capturado com a temperatura de referência
  6. Calcular a diferença absoluta |temperatura_capturada - temperatura_real|
  7. Repetir os passos 1-6 em pelo menos 3 ambientes com temperaturas diferentes (15°C, 22°C, 28°C)
- **Dados de Teste:**
  - Ciclos de coleta: 5
  - Intervalo entre ciclos: 15 segundos
  - Ambientes: 3 (temperatura fria, moderada, quente)
- **Resultado Esperado:**
  - Todos os valores de temperatura são publicados dentro do esperado
  - A diferença entre o valor capturado e o de referência é menor ou igual a ±1°C
  - Todos os valores apresentam precisão mínima de 90% (conforme RNF5)
  - Nenhum valor está fora do intervalo especificado para o sensor
  - Não há valores nulos ou inválidos
- **Prioridade:** Essencial
- **Status:** [ ] Passou | [ ] Falhou | [ ] Bloqueado

#### **TC_RF04_001 – Coleta de CO₂ (eCO₂)**

- **Requisito Associado:** RF04
- **Descrição:** Verificar se o dispositivo BIoT coleta corretamente a concentração de eCO₂ da instalação usando o sensor CJMCU-811 ou ENS160
- **Pré-condições:**
  - Dispositivo BIoT ligado e operacional
  - Sensor CJMCU-811 ou ENS160 calibrado
  - Ambiente com concentração de CO₂ estável
  - Broker MQTT ativo
  - Calibrador de CO₂ ou sensor de referência disponível
- **Passos de Teste:**
  1. Registrar a concentração de CO₂ do ambiente usando um sensor de referência
  2. Monitorar o tópico MQTT SAFE_IAQ por 5 ciclos (15 segundos × 5 = 75 segundos)
  3. Para cada ciclo, capturar a mensagem JSON e extrair o campo "co2"
  4. Comparar cada valor capturado com a concentração de referência
  5. Calcular a diferença percentual: |(co2_capturado - co2_real) / co2_real| × 100
  6. Validar se a diferença está dentro da margem de tolerância (10% conforme expectativa de precisão)
  7. Repetir em ambiente com CO₂ normal (~400 ppm) e elevado (~800 ppm)
- **Dados de Teste:**
  - Ciclos de coleta: 5
  - Intervalo entre ciclos: 15 segundos
  - Ambientes: 2 (CO₂ normal, CO₂ elevado)
- **Resultado Esperado:**
  - Todos os valores de CO₂ são publicados corretamente em ppm
  - A diferença entre valores capturados e de referência é menor que 10%
  - Todos os valores estão dentro do range do sensor (0-8000 ppm para CJMCU-811)
  - Nenhum valor está nulo ou inválido
  - A precisão atende ao mínimo de 90% (RNF5)
- **Prioridade:** Essencial
- **Status:** [ ] Passou | [ ] Falhou | [ ] Bloqueado

#### **TC_RF05_001 – Apresentação do estado da instalação**

- **Requisito Associado:** RF05
- **Descrição:** Verificar se o dispositivo BIoT apresenta o estado da instalação em relação aos parâmetros monitorados (temperatura, CO₂, número de pessoas) e emite um sinal de risco no painel sinalizador
- **Pré-condições:**
  - Dispositivo BIoT ligado com painel sinalizador acoplado
  - Sensores funcionais e calibrados
  - Limites de biossegurança carregados do broker
  - Broker MQTT ativo
  - Ambiente com temperatura, CO₂ e ocupação controláveis
- **Passos de Teste:**
  1. Configurar limites de biossegurança no broker: temp 18-25°C, CO₂ <600 ppm, pessoas <30
  2. Posicionar o ambiente dentro dos limites seguros
  3. Aguardar 2 ciclos de atualização de dados (30 segundos)
  4. Verificar se o painel sinalizador exibe cor verde (estado seguro)
  5. Monitorar mensagens MQTT para confirmar estado dentro dos limites
  6. Elevar a temperatura acima de 25°C
  7. Aguardar 2 ciclos e verificar se o painel muda para amarelo ou vermelho (estado de risco)
  8. Registrar qual parâmetro disparou o alerta
  9. Restaurar o ambiente aos limites seguros
  10. Aguardar 2 ciclos e verificar se o painel retorna ao verde
- **Dados de Teste:**
  - Limites: temp 18-25°C, CO₂ <600 ppm, ocupação <30 pessoas
  - Condição de teste: elevar temperatura para 28°C
  - Duração: 3 minutos
- **Resultado Esperado:**
  - O painel sinalizador exibe cor apropriada (verde = seguro, vermelho = risco)
  - O estado do painel muda adequadamente quando um parâmetro ultrapassa o limite
  - As mensagens MQTT refletem corretamente o estado dos parâmetros
  - O painel retorna ao estado seguro quando todos os parâmetros voltam aos limites
- **Prioridade:** Essencial
- **Status:** [ ] Passou | [ ] Falhou | [ ] Bloqueado

#### **TC_RF06_001 – Coleta de umidade**

- **Requisito Associado:** RF06
- **Descrição:** Verificar se o dispositivo BIoT coleta corretamente a umidade da instalação utilizando o sensor DHT11 ou AHT21
- **Pré-condições:**
  - Dispositivo BIoT ligado e operacional
  - Sensor DHT11 ou AHT21 calibrado
  - Ambiente com umidade estável
  - Broker MQTT ativo
  - Higrômetro de referência para validação disponível
- **Passos de Teste:**
  1. Registrar a umidade relativa do ambiente usando um higrômetro de referência
  2. Monitorar o tópico MQTT SAFE_IAQ por 5 ciclos (15 segundos × 5 = 75 segundos)
  3. Para cada ciclo, capturar a mensagem JSON e extrair o campo "umidade"
  4. Comparar cada valor capturado com a umidade de referência
  5. Calcular a diferença absoluta |umidade_capturada - umidade_real|
  6. Validar se a diferença está dentro da margem (±5% para o sensor DHT11)
  7. Repetir em ambientes com umidade diferente (30%, 50%, 70%)
- **Dados de Teste:**
  - Ciclos de coleta: 5
  - Intervalo entre ciclos: 15 segundos
  - Ambientes: 3 (umidade baixa, média, alta)
- **Resultado Esperado:**
  - Todos os valores de umidade são publicados em percentual
  - A diferença entre valores capturados e de referência é menor que 5%
  - Todos os valores estão dentro do range do sensor (0-100%)
  - A precisão atende ao mínimo de 90% (RNF5)
  - Nenhum valor está nulo ou inválido
- **Prioridade:** Essencial
- **Status:** [ ] Passou | [ ] Falhou | [ ] Bloqueado

#### **TC_RF07_001 – Coleta de VOCs**

- **Requisito Associado:** RF07
- **Descrição:** Verificar se o dispositivo BIoT coleta corretamente a medida de VOCs da instalação
- **Pré-condições:**
  - Dispositivo BIoT ligado e operacional
  - Sensor CJMCU-811 ou ENS160 calibrado (ambos coletam VOCs)
  - Ambiente com concentração de VOCs estável
  - Broker MQTT ativo
  - Sensor de referência de VOCs disponível (se possível)
- **Passos de Teste:**
  1. Registrar a concentração de VOCs do ambiente usando um sensor de referência
  2. Monitorar o tópico MQTT SAFE_IAQ por 5 ciclos (15 segundos × 5 = 75 segundos)
  3. Para cada ciclo, capturar a mensagem JSON e extrair o campo "vocs"
  4. Validar se o valor está em ppb (partes por bilhão)
  5. Comparar com o valor de referência, se disponível
  6. Registrar a faixa de valores normal para o ambiente
  7. Repetir em ambientes com diferentes concentrações de VOCs (ambiente limpo vs. com fontes de VOCs)
- **Dados de Teste:**
  - Ciclos de coleta: 5
  - Intervalo entre ciclos: 15 segundos
  - Ambientes: 2 (ambiente normal, ambiente com VOCs)
- **Resultado Esperado:**
  - Todos os valores de VOCs são publicados em ppb
  - Os valores estão dentro da faixa esperada do sensor (típico: 0-2000 ppb)
  - A precisão atende ao mínimo de 90% (RNF5)
  - Nenhum valor está nulo ou inválido
  - Os valores refletem as condições do ambiente (mais altos com fontes de VOCs)
- **Prioridade:** Importante
- **Status:** [ ] Passou | [ ] Falhou | [ ] Bloqueado

---

## 2.2 Teste de Integração

Os testes de integração verificam se os diferentes módulos do dispositivo BIoT interagem corretamente entre si e com o broker MQTT, validando o fluxo de dados e a comunicação.

### 2.2.1 Prazo para realização

Os testes de integração serão realizados após a conclusão dos testes funcionais unitários e antes dos testes de regressão. Prazo estimado: 2 semanas.

### 2.2.2 Recursos necessários

**Hardware:**
- Dispositivo BIoT completo com todos os sensores integrados
- Broker MQTT ativo
- Computador para monitoramento
- Ferramentas de captura de tráfego (Wireshark)
- Fontes de simulação de ambiente (câmaras climáticas ou equipamentos portáteis)

**Software:**
- Cliente MQTT
- Ferramentas de logging e análise de dados
- Simuladores de eventos de entrada/saída

**Pessoal:**
- 2 Testadores
- 1 Desenvolvedor para suporte

### 2.2.3 Requisitos a serem testados

- RNF1: Comunicação via MQTT
- RNF2: Formato JSON das mensagens
- RNF3: Atualização periódica de limites
- RNF8: Sincronização de relógio com NTP
- RNF9: Armazenamento local em caso de falha de conexão
- RNF10: Intervalo de publicação de dados de qualidade do ar
- RNF11: Publicação de dados de contagem quando detectado evento
- RNF12: Atualização de limites ao ligar e periodicamente

### 2.2.4 Casos de Teste

#### **TC_RNF2_001 – Formato JSON das mensagens SAFE_IAQ**

- **Requisito Associado:** RNF2
- **Descrição:** Verificar se as mensagens publicadas no tópico SAFE_IAQ estão no formato JSON correto com todos os campos obrigatórios
- **Pré-condições:**
  - Dispositivo BIoT ligado e operacional
  - Broker MQTT ativo
  - Todos os sensores funcionais
- **Passos de Teste:**
  1. Monitorar o tópico SAFE_IAQ por 3 mensagens consecutivas
  2. Capturar a carga útil (payload) de cada mensagem
  3. Validar se cada mensagem é um JSON válido
  4. Verificar presença de todos os campos obrigatórios: idBIoT, timestamp, temperatura, umidade, co2, vocs, erro
  5. Validar o formato de cada campo:
     - idBIoT: string (MAC address)
     - timestamp: string no formato dd/mm/yyyy HH:mm:ss
     - temperatura: número em °C
     - umidade: número em percentual
     - co2: número em ppm
     - vocs: número em ppb
     - erro: lista de flags ou número inteiro
  6. Verificar se não há campos vazios ou nulos inesperados
- **Dados de Teste:**
  - Número de mensagens capturadas: 3
  - Intervalo entre mensagens: 15 segundos
- **Resultado Esperado:**
  - Todas as mensagens são JSON válido
  - Todos os campos obrigatórios estão presentes em cada mensagem
  - Os valores estão nos tipos de dados corretos
  - O formato de timestamp está correto (dd/mm/yyyy HH:mm:ss)
  - Nenhuma mensagem contém campos extras não documentados
- **Prioridade:** Essencial
- **Status:** [ ] Passou | [ ] Falhou | [ ] Bloqueado

#### **TC_RNF10_001 – Intervalo de publicação de dados de qualidade do ar**

- **Requisito Associado:** RNF10
- **Descrição:** Verificar se o dispositivo BIoT publica dados de qualidade do ar no intervalo correto de 15 segundos
- **Pré-condições:**
  - Dispositivo BIoT ligado e operacional
  - Broker MQTT ativo
  - Relógio do dispositivo sincronizado
- **Passos de Teste:**
  1. Monitorar o tópico SAFE_IAQ durante 2 minutos (8 ciclos esperados)
  2. Para cada mensagem recebida, registrar o timestamp recebido e o timestamp de captura local
  3. Calcular o intervalo entre mensagens consecutivas
  4. Registrar todos os intervalos em uma tabela
  5. Calcular a média dos intervalos
  6. Validar se a média está próxima a 15 segundos (tolerância: ±2 segundos)
  7. Validar se nenhum intervalo é superior a 20 segundos
- **Dados de Teste:**
  - Duração do monitoramento: 2 minutos
  - Intervalo esperado: 15 segundos
  - Tolerância: ±2 segundos
- **Resultado Esperado:**
  - Todas as 8 mensagens são recebidas dentro do intervalo esperado
  - A média dos intervalos é 15 ± 1 segundo
  - Nenhuma mensagem é perdida ou duplicada
  - Não há intervalos anormalmente longos (>20 segundos)
- **Prioridade:** Essencial
- **Status:** [ ] Passou | [ ] Falhou | [ ] Bloquea

#### **TC_RNF6_001 – Retomada automática após perda de energia**

- **Requisito Associado:** RNF6
- **Descrição:** Verificar se o dispositivo BIoT retoma automaticamente suas atividades após uma interrupção no fornecimento de energia
- **Pré-condições:**
  - Dispositivo BIoT ligado, operacional e publicando dados normalmente
  - Broker MQTT ativo
  - Todos os sensores funcionais
  - Tempo de teste: 10 minutos
- **Passos de Teste:**
  1. Monitorar o tópico SAFE_IAQ por 2 ciclos de publicação (30 segundos)
  2. Registrar que o dispositivo está operacional
  3. Desligar abruptamente a fonte de alimentação
  4. Aguardar 10 segundos
  5. Religar a fonte de alimentação
  6. Monitorar o tópico SAFE_IAQ nos próximos 2 minutos
  7. Verificar se as publicações retomam dentro de 30 segundos
  8. Validar se os dados publicados após a retomada são válidos
  9. Registrar se houve perda de dados ou mensagens corrompidas
- **Dados de Teste:**
  - Duração do desligamento: 10 segundos
  - Tempo máximo esperado para retomada: 30 segundos
  - Número de ciclos monitorados antes: 2
  - Número de ciclos monitorados após: 8
- **Resultado Esperado:**
  - O dispositivo se reinicia automaticamente quando a energia é restaurada
  - As publicações retomam dentro de 30 segundos
  - Nenhum dado é perdido ou corrompido
  - O dispositivo retorna ao estado operacional normal
- **Prioridade:** Essencial
- **Status:** [ ] Passou | [ ] Falhou | [ ] Bloqueado

#### **TC_RNF7_001 – Reconexão automática à internet**

- **Requisito Associado:** RNF7
- **Descrição:** Verificar se o dispositivo BIoT reconecta-se automaticamente à rede Wi-Fi e ao broker quando a conexão é perdida
- **Pré-condições:**
  - Dispositivo BIoT ligado e operacional
  - Broker MQTT ativo
  - Roteador Wi-Fi com controle de desconexão disponível
- **Passos de Teste:**
  1. Confirmar que o dispositivo está conectado e publicando normalmente (2 ciclos)
  2. Interromper a conexão Wi-Fi (desativar roteador ou isolar o dispositivo)
  3. Monitorar o comportamento do dispositivo por 1 minuto
  4. Restaurar a conexão Wi-Fi
  5. Monitorar a retomada das publicações
  6. Registrar o tempo decorrido para reconexão (desde o restabelecimento até a primeira publicação)
  7. Validar se os dados publicados após reconexão são válidos
- **Dados de Teste:**
  - Duração da desconexão: 1 minuto
  - Tempo máximo esperado para reconexão e primeira publicação: 60 segundos
- **Resultado Esperado:**
  - O dispositivo detecta a perda de conexão
  - Tenta se reconectar automaticamente quando a rede volta
  - Retoma as publicações dentro de 60 segundos
  - Nenhum dado é perdido durante o processo de reconexão
- **Prioridade:** Essencial
- **Status:** [ ] Passou | [ ] Falhou | [ ] Bloqueado

#### **TC_RNF9_001 – Armazenamento local em caso de falha de conexão**

- **Requisito Associado:** RNF9
- **Descrição:** Verificar se o dispositivo BIoT armazena dados localmente quando há falha de conexão e os envia ao broker quando a conexão é restabelecida
- **Pré-condições:**
  - Dispositivo BIoT ligado e operacional
  - Broker MQTT ativo
  - Memória local do dispositivo disponível
  - Capacidade de interromper a conexão com o broker
- **Passos de Teste:**
  1. Confirmar que o dispositivo está publicando normalmente (1 ciclo)
  2. Interromper a conexão com o broker (manter Wi-Fi ativo)
  3. Aguardar 3 ciclos de coleta de dados (45 segundos) sem conexão com o broker
  4. Registrar que durante este período, nenhuma publicação ocorre no broker
  5. Restaurar a conexão com o broker
  6. Monitorar o tópico SAFE_IAQ nos próximos 2 minutos
  7. Verificar se os dados que foram coletados durante a desconexão são publicados
  8. Validar se os dados enviados têm timestamps corretos (relativos ao momento da coleta)
- **Dados de Teste:**
  - Duração da desconexão do broker: 45 segundos (3 ciclos)
  - Número esperado de ciclos armazenados: 3
- **Resultado Esperado:**
  - O dispositivo coleta dados mesmo sem conexão com o broker
  - Os dados são armazenados localmente
  - Quando a conexão é restabelecida, todos os dados armazenados são enviados
  - Os timestamps dos dados refletem o momento original da coleta
  - Nenhum dado é perdido durante o processo
- **Prioridade:** Essencial
- **Status:** [ ] Passou | [ ] Falhou | [ ] Bloqueado

#### **TC_RNF13_001 – Coleta contínua durante a semana**

- **Requisito Associado:** RNF13
- **Descrição:** Verificar se o dispositivo BIoT coleta e envia informações continuamente durante a semana, seguindo os intervalos especificados
- **Pré-condições:**
  - Dispositivo BIoT ligado continuamente por 7 dias
  - Broker MQTT ativo durante todo o período
  - Todos os sensores funcionais
  - Monitoramento contínuo dos tópicos MQTT
- **Passos de Teste:**
  1. Energizar o dispositivo e iniciá-lo no domingo
  2. Monitorar os tópicos SAFE_IAQ e SAFE_ENTRY_FLOW continuamente
  3. Registrar o número de publicações esperadas por dia: 5760 mensagens SAFE_IAQ (1 a cada 15 seg × 86400 seg/dia)
  4. Registrar o número real de mensagens recebidas
  5. Calcular a taxa de sucesso: (mensagens_reais / mensagens_esperadas) × 100
  6. Registrar qualquer interrupção ou anomalia
  7. Avaliar a taxa de sucesso no final de 7 dias
- **Dados de Teste:**
  - Duração do teste: 7 dias
  - Intervalo de publicação: 15 segundos para SAFE_IAQ
  - Mensagens esperadas: ~5760 por dia
- **Resultado Esperado:**
  - O dispositivo publica continuamente durante os 7 dias
  - A taxa de sucesso é superior a 99% (perda máxima aceitável: ~57 mensagens em 7 dias)
  - Não há interrupções não planejadas
  - Todos os dados publicados são válidos
- **Prioridade:** Essencial
- **Status:** [ ] Passou | [ ] Falhou | [ ] Bloqueado

#### **TC_RNF22_001 – Reportagem de falhas através de etiquetas de erro**

- **Requisito Associado:** RNF22
- **Descrição:** Verificar se o dispositivo BIoT reporta corretamente falhas de coleta de medidas através de etiquetas de erro no campo "erro" das mensagens MQTT
- **Pré-condições:**
  - Dispositivo BIoT ligado
  - Broker MQTT ativo
  - Capacidade de desativar sensores ou simular falhas
- **Passos de Teste:**
  1. Confirmar operação normal do dispositivo (erro = 0)
  2. Desativar ou desconectar o sensor DHT11/AHT21 (sensor de temperatura/umidade)
  3. Monitorar a próxima publicação de SAFE_IAQ
  4. Verificar se o campo "erro" contém a flag ERROR_TEMP (2) e ERROR_HUMID (4)
  5. Validar se o valor de erro é 6 (2 + 4 em binário)
  6. Reconectar o sensor e monitorar se o erro volta a 0
  7. Repetir para o sensor CJMCU-811/ENS160 (verificar flag ERROR_CO2 = 8 ou ERROR_TVOC = 16)
  8. Testar múltiplas falhas simultâneas (ex: temperatura e CO2)
  9. Validar se os erros se combinam corretamente em binário
- **Dados de Teste:**
  - Sensores a desativar: DHT11, CJMCU-811
  - Flags esperadas: ERROR_TEMP (2), ERROR_HUMID (4), ERROR_CO2 (8), ERROR_TVOC (16)
  - Combinações: erro único, erro duplo, erro triplo
- **Resultado Esperado:**
  - Cada falha é corretamente reportada com a flag correspondente
  - Múltiplas falhas são reportadas como a soma binária das flags
  - O campo "erro" é 0 quando não há falhas
  - As flags são mantidas enquanto a falha persiste
  - As flags são limpas quando a falha é resolvida
- **Prioridade:** Importante
- **Status:** [ ] Passou | [ ] Falhou | [ ] Bloqueado

---

## 3. Resultados dos Testes

Esta seção será preenchida conforme os testes forem executados. Para cada tipo de teste e caso de teste, será documentado:

- Data de execução
- Resultado (Passou/Falhou/Bloqueado)
- Observações e problemas encontrados
- Ações corretivas tomadas
- Data da resolução

### Resumo de Testes de Regressão

| ID do Caso | Descrição | Status | Data | Observações |
|-----------|-----------|--------|------|-------------|
| TC_REG_001 | Suite de regressão - Testes críticos | [ ] Passou [ ] Falhou | | |

### Resumo de Testes Não Funcionais

| ID do Caso | Descrição | Status | Data | Observações |
|-----------|-----------|--------|------|-------------|
| TC_RNF6_001 | Retomada após falha de energia | [ ] Passou [ ] Falhou | | |
| TC_RNF7_001 | Reconexão automática | [ ] Passou [ ] Falhou | | |
| TC_RNF9_001 | Armazenamento local | [ ] Passou [ ] Falhou | | |
| TC_RNF13_001 | Coleta contínua semanal | [ ] Passou [ ] Falhou | | |
| TC_RNF22_001 | Reportagem de erros | [ ] Passou [ ] Falhou | | |

---

## 4. Critérios de Entrada

Para iniciar os testes de regressão e não funcional, os seguintes critérios devem ser atendidos:

- [ ] Todos os testes funcionais foram executados e aprovados
- [ ] Todos os testes de integração foram executados e aprovados
- [ ] Nenhum defeito crítico está pendente
- [ ] O firmware foi compilado com sucesso e está pronto para teste
- [ ] Ambiente de teste está totalmente configurado e validado

## 5. Critérios de Saída

Para considerar os testes de regressão e não funcional como concluídos com sucesso:

- [ ] Todos os casos de teste essenciais foram executados
- [ ] Taxa de sucesso mínima de 95% em todos os testes
- [ ] Taxa de sucesso em testes de longa duração (RNF13) ≥ 99%
- [ ] Nenhum defeito crítico está aberto
- [ ] Número aceitável de defeitos maiores: máximo 2 abertos
- [ ] Todos os defeitos corrigidos foram validados por regressão
- [ ] Documentação de testes está completa
- [ ] Concordância da equipe sobre a qualidade do produto para deploy
- [ ] Testes de regressão final passaram na versão de produçãos