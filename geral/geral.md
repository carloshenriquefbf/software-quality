# 1. Visão

Este documento apresenta uma visão geral do Sistema SAFE. Inicialmente, descreve-se o escopo do projeto, bem como o objetivo e os itens excluídos, a fim de deixar claro o propósito do sistema de software proposto. Além disso, este documento descreve o time de desenvolvimento e seus respectivos papéis no sistema de software. Os requisitos (funcionais e não funcionais) também estão especificados na Seção 2.

## 1.1. Escopo do Projeto

Em meio à pandemia da COVID-19, a UFRJ ficou impossibilitada de manter suas atividades presenciais em todas as dependências, o que trouxe problemas tanto para os estudantes da universidade quanto para os docentes e terceirizados que nela atuavam.

Como parte do plano de tomada de atividades presenciais na UFRJ, este projeto tem o propósito de apoiar o monitoramento das condições de uso das instalações da UFRJ, considerando os níveis de risco e as condições do ambiente (temperatura, nível de CO2 e número de pessoas por instalação), conforme definido no Guia de Biossegurança da UFRJ.

Nesse contexto, o projeto **COVID SAFE Classrooms** tem como objetivos:
- Aplicar regras do Guia de Biossegurança às instalações físicas da UFRJ para ocupação segura contra a COVID-19;
- Garantir monitoramento dos riscos atribuídos às condições de utilização e ocupação das instalações;
- Garantir atuação técnica e gerencial preventiva e de mitigação dos riscos associados às condições monitoradas.

## 1.2. Escopo Não Incluído no Projeto

Esse sistema apenas fornece as informações sobre os indicadores previstos no Guia de biossegurança para as instalações da UFRJ. Sendo assim, **não oferece qualquer funcionalidade para evitar ou reduzir o fluxo de pessoas** em cada instalação.

## 1.4. Glossário

| Termo | Descrição |
|-------|-----------|
| AHT21 | Sensor de Temperatura e Umidade |
| AP | Quantidade de pessoas numa instalação específica |
| Arquitetura dual-core | Arquitetura com dois cores de processamento oferecida pelo ESP32 |
| BioT | Dispositivo IoT composto por sensor de temperatura, sensor de movimento, sensor de CO2. Suas propriedades são: Id, versão do software, porta e data de instalação. |
| BIoT | O BIoT é um dispositivo IoT voltado à coleta de dados ambientais internos para biossegurança, integrando sensores de qualidade do ar e de contagem de pessoas para monitorar CO₂, temperatura, umidade e ocupação dos ambientes. |
| Bloco | Espaço físico pertencente a uma ou mais unidades que possui um conjunto de instalações. Pode ter um ou mais andares. |
| Broker | Agente intermediário que utiliza o protocolo MQTT responsável pela comunicação entre os dados capturados pelo dispositivo IoT e o sistema. Utilizado o Rabbitmq |
| C++ | Linguagem de programação |
| CJMCU-811 | Sensor de CO2 |
| CO₂ | Dióxido de carbono. Unidade de medida: ppm. |
| CT | CT-2 Centro de Gestão Tecnológica |
| DHT11 | Sensor de Temperatura e Umidade |
| Docker | Tecnologia para encapsulamento de aplicações |
| ENS160 | Sensor de CO2 |
| ESP32 | Processador do BIoT |
| Estados de solicitações de limpeza ou manutenção | “Em andamento”, “Aguardando atendimento”, “Atendida” |
| Estados de utilização das instalações no SAFE Dashboard | “Liberado”, “Não liberado” |
| Estados de utilização das instalações no SAFE Manager | “Aguardando limpeza”, “Aguardando manutenção”, “Aguardando limpeza e manutenção”, “Em uso”, "Disponível para uso” |
| FreeRTOS | Sistema operacional multi-core utilizado no BIoT |
| HC-SR04 | Sensor de presença ultrassônico |
| Instalação | Qualquer ambiente utilizado na UFRJ para o desenvolvimento de atividades presenciais deve possuir as seguintes informações: localização, unidade, nível de risco, estado de utilização, dispositivo BIoT associado e tipo de instalação. |
| INTERVALO_ATUALIZACAO_DADOS_BROKER | Corresponde ao intervalo entre acessos sucessivos ao broker, a fim de coletar novos dados transmitidos pelos dispositivos IoT. Seu valor é de 15 segundos. |
| INTERVALO_ATUALIZAÇÃO_DADOS_CO2 | Corresponde ao intervalo de coleta das medidas de CO2 e de VOCS. O sensor CJCMU 811 tem restrição temporal. Este intervalo é de 5 segundos. |
| INTERVALO_ATUALIZACAO_DADOS_TEMPERATURA | Corresponde ao intervalo de coleta das medidas de temperatura e de umidade. O sensor DTH11 tem restrições temporais. Este intervalo deve ser de 7,5 segundos. |
| INTERVALO_ATUALIZACAO_ESTADO | Refere-se ao intervalo de tempo entre as verificações dos estados das instalações. Esse intervalo está definido em 10 minutos. |
| INTERVALO_ATUALIZACAO_LIMITES | Refere-se ao intervalo de tempo entre os envios dos limites de biossegurança para um dispositivo BIoT em uma determinada instalação. Esse intervalo está configurado para 30 minutos. |
| INTERVALO_ATUALIZACAO_NOTIFICACOES | Corresponde ao intervalo entre as buscas por novas notificações e a manutenção da conexão Wi‑Fi do sistema. Seu valor é de 10 segundos. |
| INTERVALO_ATUALIZAÇÃO_NUMERO_PESSOAS | Corresponde ao intervalo de coleta dos eventos de entrada e de saída de pessoas na instalação. Os sensores HC SR04 devem estar posicionados adequadamente. Este intervalo deve ser de 1 segundo. |
| INTERVALO_ATUALIZAR_DADOS | Corresponde ao intervalo entre acessos sucessivos ao SAFE Manager, para coletar novos dados disponíveis. Seu valor é de 15 segundos. |
| Localização | Uma localização segue uma estrutura hierárquica que descreve um espaço físico. O nível mais alto da hierarquia é definido pelo centro, seguido pelo bloco e pelo andar. Seu propósito é permitir a identificação do local físico onde uma instalação se encontra. |
| MQTT | Message Queuing Telemetry Transport (MQTT) é um protocolo leve de mensagens para sensores e pequenos dispositivos móveis, otimizado para redes TCP/IP. |
| MQTT_TOPICO | Indica o tópico principal a ser usado para organizar os dados dos dispositivos. Neste caso o valor default é “SAFE”. |
| Parâmetros de funcionamento | Corresponde aos parâmetros: frequência de leitura dos BIoT, período de captura de eventos do sistema, frequência de leitura dos limites de biossegurança das instalações cadastradas. |
| PERIODO_OBSERVACAO_HISTORICO_DADOS | Corresponde ao período em que os dados históricos do sistema serão exibidos. Como padrão, o histórico de dados deve corresponder às últimas QUATRO horas de coleta. |
| PPB | "partes por bilhão" (parts per billion) é uma unidade de concentração usada para medir quantidades muito baixas de um gás no ar ou em uma solução. |
| PPM | "partes por milhão" (Parts Per Million) é uma unidade de medida que indica a concentração de dióxido de carbono no ar. |
| Protocolo MQTT | Protocolo de comunicação utilizado pelo BIoT |
| Risco de contaminação por Covid-19 | Classificação de Risco de contaminação por Covid-19, considerando a temperatura, o nível de CO2 e o número de pessoas em uma determinada instalação, conforme descrito no Guia de Biossegurança. |
| TEMPO_MAXIMO_ANTES_ALERTA_DASHBOARD | Refere-se ao tempo máximo permitido para que o Dashboard receba novos dados. Caso o Dashboard não receba novas informações em 5 minutos, um alerta será exibido. |
| TEMPO_MAXIMO_INATIVIDADE | Seu valor é de 1 hora. |
| Temperatura (TEMP) | Temperatura do ambiente fornecida pelo sensor em graus Celsius (°C). |
| Tipo de instalação | Podem assumir os seguintes valores: sala de aula, laboratório, sala de reunião, auditório, sala administrativa, refeitório, sala de convivência e biblioteca. |
| Unidade | Representa a unidade que administra a instalação (por exemplo: COPPE). |
| VOCs | Refere-se à quantidade de compostos orgânicos voláteis (VOCs) fornecida pelo sensor em pbm. VOCs são substâncias que contêm carbono em sua composição e apresentam ponto de ebulição entre 50 °C e 260 °C. São considerados poluentes perigosos e alguns deles possuem propriedades tóxicas e potencial carcinogênico. |

## 1.5. Definição de papéis

| Papel | Descrição |
|-------|-----------|
| **Administrador do Sistema** | É o superusuário do sistema. Possui privilégios para realizar as ações de operação inicial do sistema e garantir seu funcionamento, tais como gerenciar as instalações, os gestores das instalações, acompanhar notificações do sistema, como problemas identificados que impedem o correto funcionamento, e gerenciar os dispositivos disponíveis nas instalações. |
| **Gestor das Instalações** | Possui privilégios para acompanhar notificações do sistema, gerenciar usuários (Equipe de Manutenção, Equipe de Limpeza, Staff). Além disso, podem gerenciar instalações, limites de biossegurança e solicitações de uso das instalações, incluindo serviços de limpeza e manutenção. |
| **Equipe de Manutenção** | Envolve os funcionários de manutenção. Eles têm privilégios que lhes permitem receber e atender solicitações de manutenção de instalações. Representada pelo Gerente da Manutenção. |
| **Equipe de Limpeza** | Envolve os funcionários de limpeza. Eles têm privilégios que lhes permitem receber e atender solicitações de limpeza das instalações. Representada pelo gerente da limpeza. |
| **Staff** | Envolve professores e demais funcionários autenticados no sistema. Estes têm permissão para solicitar o uso de instalações, bem como serviços de limpeza e manutenção. |
| **Usuário não-cadastrado** | Envolve todos os demais usuários potenciais do sistema, como alunos e visitantes. A eles é permitido apenas o acesso à interface do Dashboard. |

## 1.6. Regras de Negócio

As regras de negócio, quando pertinentes, são apresentadas em conjunto com seus respectivos requisitos. O **Guia de ações de biossegurança para a resposta à pandemia de COVID-19 no âmbito da UFRJ** apresenta os princípios motivacionais e comportamentais abordados no projeto.

# 2. SUBSISTEMAS ASSOCIADOS

Em relação às funcionalidades do sistema disponibilizadas, o Sistema SAFE está representado na Figura 1 abaixo, sendo formado pelos seguintes subsistemas principais:

1.  **SAFE BIoT** - dispositivo IoT para a coleta e o envio de dados das instalações.
2.  **Broker de Mensagem** – subsistema responsável pela comunicação entre os SAE BIoT e o SAFE Manager. Este sistema utiliza o protocolo MQTT.
3.  **SAFE Dashboard** – painel de monitoramento responsável pela exibição dos dados coletados pelo SAFE Manager das instalações. O subsistema do dashboard tem comunicação direta com o subsistema do SAFE Manager.
4. **SAFE Manager** - subsistema central responsável pela configuração e gerenciamento de usuários e dispositivos SAFE BIoT em uma determinada instalação (sala, laboratório, auditório, secretaria, entre outras). Ele se comunica com o Subsistema Broker (Broker de Mensagem / MQTT Broker) para receber as Mensagens Criptografadas enviadas pelos dispositivos IoT e disponibiliza os dados para o SAFE Dashboard.

Subsistema SAFE (Dispositivo IoT): Coleta dados do ambiente e os envia, criptografados, via protocolo MQTT.

Subsistema Broker (MQTT Broker / Broker de Mensagem): Atua como intermediário de mensagens, recebendo dados criptografados do dispositivo IoT e repassando-os para o Manager.

Subsistema Manager (Servidor): Processa os dados recebidos do broker, gerencia a lógica de negócio, usuários e dispositivos. Serve como backend para o Dashboard.

Subsistema Dashboard (Painel de Monitoramento / Interface de Gerenciamento): Fornece a interface visual (frontend) para monitoramento em tempo real dos indicadores e para o gerenciamento das instalações, comunicando-se diretamente com o Manager.

# 3. SUBSISTEMA MANAGER

O Manager é o componente sistêmico central do SAFE, responsável por orquestrar as ações de monitoramento das instalações. Por meio do Manager, é possível realizar operações de cadastramento e de gestão de instalações e dispositivos BioT. Toda a comunicação com os BIoTs é feita pelo Manager, que utiliza o broker MQTT para interagir. O Manager mantém um repositório de dados que garante a preservação do registro de eventos e a correta interação com o SAFE Dashboard.

# 4. SUBSISTEMA SAFE BIOT

O BIoT é um dispositivo IoT projetado para coletar dados ambientais em instalações internas, com foco em parâmetros de biossegurança, conforme mostrado na Figura 1. Ele é composto por dois módulos principais de sensoriamento: o módulo de qualidade do ar, que utiliza sensores CJMCU-811 (ou ENS160) e DHT11 (ou AHT21) para medir níveis de CO₂, temperatura e umidade; e o módulo de contagem de pessoas, baseado em sensores ultrassônicos HC-SR04, que permite monitorar a entrada e a saída de pessoas nos ambientes.

Para lidar com a periodicidade de funcionamento distinta entre os módulos, o dispositivo utiliza um microcontrolador ESP32 com suporte ao FreeRTOS, devido à sua arquitetura dual-core, que permite que cada núcleo fique responsável por um dos módulos.

As rotinas do dispositivo foram desenvolvidas em C++, incluindo um componente próprio de criptografia responsável por proteger os dados antes de sua transmissão. Os dados coletados são enviados pela rede Wi-Fi, por meio do protocolo MQTT, para um broker de mensagens, garantindo a comunicação com os demais subsistemas da arquitetura.

# 5. SUBSISTEMA SAFE DASHBOARD

O Dashboard é o Painel de Monitoramento do sistema SAFE, visando viabilizar o acompanhamento contínuo dos indicadores de biossegurança dos ambientes monitorados pelo sistema, conforme mostra a Figura 1. Esse painel foi desenvolvido com tecnologias front-end, como React, e oferece uma interface visual para a visualização contínua dos dados, a emissão de alertas em situações críticas e apoio à mitigação de riscos nos espaços.

Além disso, o Dashboard serve também como Interface de Gerenciamento, que permite aos usuários solicitarem remotamente serviços de limpeza ou manutenção, além de informar o estado atual do ambiente, especificando se ele está bloqueado ou liberado para uso. Com base nessas informações, o SAFE IoT gerencia e comunica dois estados possíveis para cada instalação: disponível e indisponível. Quando o ambiente estiver disponível, o sistema notifica os usuários interessados sobre sua liberação. Quando está indisponível, além da notificação, o sistema informa o motivo, que pode estar relacionado à necessidade de limpeza, à manutenção em andamento ou ao bloqueio operacional.

# 6. CONSIDERAÇÕES

## Escopo Não Incluído no Projeto

Esse sistema apenas fornece as informações sobre os indicadores previstos no Guia de Biossegurança para as instalações da UFRJ. Sendo assim, **não oferece qualquer funcionalidade para evitar ou reduzir o fluxo de pessoas** em cada instalação.

## Impactos:

- **Monitoramento contínuo de biossegurança em ambientes fechados:** mesmo após a pandemia, o monitoramento de temperatura, umidade, CO₂ e número de pessoas permanece necessário para garantir condições saudáveis e confortáveis em ambientes com circulação de pessoas, como salas de aula, laboratórios e ambientes hospitalares.
- **Automatização de processos operacionais:** o sistema permite não só a solicitação de serviços (como limpeza ou manutenção), mas também o bloqueio ou liberação de ambientes com base em critérios objetivos de biossegurança, reduzindo decisões manuais e aumentando a eficiência.

## Expectativas:

- **Fornecer dados de monitoramento em tempo real do ambiente:** espera-se que o painel exiba, de forma clara, as informações de temperatura, umidade, nível de CO₂ e número de pessoas no ambiente.
- **Emitir um alerta inteligente para condições críticas:** o sistema deve notificar automaticamente quando os níveis definidos pelo usuário ultrapassarem o limite seguro.

## Público alvo:

- Profissionais da área de saúde, segurança do trabalho e biossegurança;
- Equipes de manutenção, limpeza;
- Usuários dos espaços monitorados (colaboradores, estudantes, pacientes, visitantes).