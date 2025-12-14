# Plano de Testes do Subsistema SAFE Dashboard – versão 1.0.0

---

## Histórico de Revisão

| **Data** | **Versão** | **Descrição** | **Autor** |
|----------|------------|---------------|-----------|
| 15/12/2024 | 1.0.0 | Criação inicial do plano de testes para o SAFE Dashboard | Analista de Testes Sênior |

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
      2.1.1. [Prazo para realização](#211-prazo-para-realização)
      2.1.2. [Recursos necessários](#212-recursos-necessários)
      2.1.3. [Requisitos a serem testados](#213-requisitos-a-serem-testados)
      2.1.4. [Casos de Teste](#214-casos-de-teste)
   2.2. [Teste de Integração](#22-teste-de-integração)
      2.2.1. [Prazo para realização](#221-prazo-para-realização)
      2.2.2. [Recursos necessários](#222-recursos-necessários)
      2.2.3. [Requisitos a serem testados](#223-requisitos-a-serem-testados)
      2.2.4. [Casos de Teste](#224-casos-de-teste)
   2.3. [Teste de Usabilidade](#23-teste-de-usabilidade)
      2.3.1. [Prazo para realização](#231-prazo-para-realização)
      2.3.2. [Recursos necessários](#232-recursos-necessários)
      2.3.3. [Requisitos a serem testados](#233-requisitos-a-serem-testados)
      2.3.4. [Casos de Teste](#234-casos-de-teste)
   2.4. [Teste de Segurança](#24-teste-de-segurança)
      2.4.1. [Prazo para realização](#241-prazo-para-realização)
      2.4.2. [Recursos necessários](#242-recursos-necessários)
      2.4.3. [Requisitos a serem testados](#243-requisitos-a-serem-testados)
      2.4.4. [Casos de Teste](#244-casos-de-teste)
3. [Resultados dos Testes](#3-resultados-dos-testes)

---

# 1. Introdução

### 1.1 Finalidade
Este documento tem como finalidade planejar e documentar a execução dos testes do subsistema **SAFE Dashboard**, garantindo que todas as funcionalidades estejam em conformidade com os requisitos funcionais e não funcionais especificados, além de validar a usabilidade, segurança e integração com os demais subsistemas do Sistema SAFE.

### 1.2 Escopo
O escopo deste plano de testes abrange exclusivamente o **subsistema SAFE Dashboard**, conforme descrito no documento de requisitos associado. Serão testadas as funcionalidades de exibição de dados, alertas, atualização de informações, comunicação com o SAFE Manager, responsividade da interface e comportamentos em condições de falha.

### 1.3 Definições, Acrônimos, e Abreviações

| Termo | Descrição |
|-------|-----------|
| BIoT | Dispositivo IoT de biossegurança |
| CO₂ | Dióxido de carbono (medido em ppm) |
| Dashboard | Interface visual do Sistema SAFE |
| Manager | Subsistema central SAFE Manager |
| MQTT | Protocolo de comunicação leve para IoT |
| RF | Requisito Funcional |
| RNF | Requisito Não Funcional |
| VOCs | Compostos Orgânicos Voláteis (medidos em ppb) |

### 1.4 Referências

| **Título** | **Versão** | **Data** | **Onde pode ser obtido** |
|------------|------------|----------|--------------------------|
| Requisitos do Sistema SAFE Dashboard | 1.0 | 15/12/2024 | requisitos-dashboard.md |
| Visão Geral do Sistema SAFE | 1.0 | 15/12/2024 | geral.md |
| Plano de Testes do Sistema Dashboard (Exemplo) | 1.0 | 15/12/2024 | plano-de-testes-dashboard.md |
| Template para Plano de Testes | 1.0.0 | 15/12/2024 | template.md |

### 1.5 Visão geral
Este documento está organizado em três seções principais: **Introdução**, que contextualiza o plano; **Estratégia de Teste**, que detalha os tipos de testes a serem realizados com seus respectivos casos de teste; e **Resultados dos Testes**, que será preenchido com os resultados obtidos durante a execução.

# 2. Estratégia de Teste

Serão realizados os seguintes tipos de teste, organizados por nível e momento adequado:

## 2.1 Teste Funcional

### 2.1.1 Prazo para realização
Durante todo o ciclo de desenvolvimento, com foco após a finalização de cada funcionalidade e antes da entrega de cada release.

### 2.1.2 Recursos necessários
- Ambiente de desenvolvimento com Dashboard implantado
- SAFE Manager ativo e configurado
- Dados simulados de BIoT
- Navegadores web (Chrome, Firefox, Edge, Safari)
- Ferramenta de captura de tráfego (ex: Wireshark, DevTools)

### 2.1.3 Requisitos a serem testados
RF01 a RF12

### 2.1.4 Casos de Teste

| ID Caso de Teste | Requisito Associado | Descrição | Passos de Teste | Resultado Esperado | Prioridade |
|------------------|---------------------|-----------|----------------|-------------------|------------|
| TC_RF01 | RF01 | Verificar exibição da temperatura por instalação | 1. Acessar o Dashboard sem autenticação<br>2. Selecionar uma instalação com dispositivo vinculado<br>3. Verificar card da instalação | Temperatura exibida em °C | Essencial |
| TC_RF02 | RF02 | Verificar exibição de CO₂ em ppm | 1. Acessar o Dashboard<br>2. Verificar card de instalação com dispositivo | CO₂ exibido em ppm | Essencial |
| TC_RF03 | RF03 | Verificar exibição do número de pessoas | 1. Acessar o Dashboard<br>2. Verificar card da instalação | Número de pessoas exibido | Essencial |
| TC_RF04 | RF04 | Verificar classificação de risco | 1. Acessar o Dashboard<br>2. Verificar se há indicador de risco (ex: cor, ícone) | Classificação de risco visível conforme Guia de Biossegurança | Essencial |
| TC_RF05 | RF05 | Verificar estado de utilização | 1. Acessar o Dashboard<br>2. Verificar ícone/legenda de estado | Estado exibido: "bloqueado (em manutenção)", "bloqueado (em limpeza)" ou "liberado" | Essencial |
| TC_RF06 | RF06 | Verificar localização e unidade | 1. Acessar o Dashboard<br>2. Verificar card da instalação | Localização e unidade exibidas | Importante |
| TC_RF07 | RF07 | Verificar gráficos de evolução na última hora | 1. Acessar o Dashboard<br>2. Clicar em instalação para ver detalhes<br>3. Verificar gráficos lineares | Gráficos de temperatura, CO₂, umidade, VOCs e pessoas exibidos com intervalo de 2 min | Importante |
| TC_RF08 | RF08 | Verificar valores máximos na última hora | 1. Acessar detalhes da instalação<br>2. Verificar seção de máximos | Valores máximos de cada medida exibidos | Importante |
| TC_RF09 | RF09 | Verificar exibição de umidade | 1. Acessar Dashboard<br>2. Verificar card da instalação | Umidade exibida em percentual | Essencial |
| TC_RF10 | RF10 | Verificar exibição de VOCs | 1. Acessar Dashboard<br>2. Versificar card da instalação | VOCs exibidos em ppb | Importante |
| TC_RF11 | RF11 | Verificar alerta de limite atingido | 1. Simular valor acima do limite no Manager<br>2. Acessar Dashboard<br>3. Verificar alerta visual (ícone ⚠) | Alerta exibido quando limite for ultrapassado | Essencial |
| TC_RF12 | RF12 | Verificar legenda no rodapé | 1. Acessar Dashboard<br>2. Rolar até o rodapé | Legenda com cores e ícones (→ bloqueado, → liberado, ⚠ limite) visível | Importante |

## 2.2 Teste de Integração

### 2.2.1 Prazo para realização
Após a integração do Dashboard com o SAFE Manager, antes dos testes de aceitação.

### 2.2.2 Recursos necessários
- Ambiente integrado (Dashboard + Manager + Broker)
- BIoT simulado ou real
- Ferramentas de monitoramento de API (ex: Postman, Insomnia)

### 2.2.3 Requisitos a serem testados
RNF01, RNF02, RNF03, RNF05, RNF06, RNF07, RNF15

### 2.2.4 Casos de Teste

| ID Caso de Teste | Requisito Associado | Descrição | Passos de Teste | Resultado Esperado | Prioridade |
|------------------|---------------------|-----------|----------------|-------------------|------------|
| TC_RNF01 | RNF01 | Verificar comunicação com API do Manager | 1. Monitorar requisições do Dashboard<br>2. Verificar chamadas às rotas do Manager | Comunicação bem-sucedida, respostas JSON válidas | Essencial |
| TC_RNF02 | RNF02 | Verificar obtenção de limites de biossegurança | 1. Alterar limites no Manager<br>2. Atualizar Dashboard<br>3. Verificar se limites foram atualizados | Limites exibidos correspondem aos configurados no Manager | Essencial |
| TC_RNF05 | RNF05 | Verificar atualização de dados a cada INTERVALO_ATUALIZAR_DADOS | 1. Registrar hora da última atualização<br>2. Aguardar 15 segundos<br>3. Verificar nova requisição | Dados atualizados a cada 15 segundos | Essencial |
| TC_RNF06 | RNF06 | Verificar atualização de limites a cada INTERVALO_ATUALIZACAO_LIMITES | 1. Alterar limite no Manager<br>2. Aguardar 30 minutos<br>3. Verificar Dashboard | Limites atualizados após 30 minutos | Importante |
| TC_RNF07 | RNF07 | Verificar atualização de estados a cada INTERVALO_ATUALIZACAO_ESTADO | 1. Alterar estado no Manager<br>2. Aguardar 10 minutos<br>3. Verificar Dashboard | Estado atualizado após 10 minutos | Importante |

## 2.3 Teste de Usabilidade

### 2.3.1 Prazo para realização
Após a estabilização da interface, antes do release final.

### 2.3.2 Recursos necessários
- Dispositivos variados (desktop, tablet, smartphone)
- Usuários representativos (staff, administradores, visitantes)
- Checklist de usabilidade

### 2.3.3 Requisitos a serem testados
RNF13, RNF14

### 2.3.4 Casos de Teste

| ID Caso de Teste | Requisito Associado | Descrição | Passos de Teste | Resultado Esperado | Prioridade |
|------------------|---------------------|-----------|----------------|-------------------|------------|
| TC_USB01 | RNF13 | Verificar responsividade em diferentes telas | 1. Acessar Dashboard em desktop, tablet e smartphone<br>2. Redimensionar janela do navegador | Interface se adapta sem quebras ou sobreposições | Essencial |
| TC_USB02 | RNF14 | Verificar organização em cards | 1. Acessar Dashboard com múltiplas instalações<br>2. Verificar disposição dos cards | Cada instalação em card individual, organizados de forma clara | Importante |

## 2.4 Teste de Segurança

### 2.4.1 Prazo para realização
Após a implementação das funcionalidades de acesso e comunicação.

### 2.4.2 Recursos necessários
- Ferramentas de análise de rede
- Ambiente de testes isolado

### 2.4.3 Requisitos a serem testados
RNF04, RNF08, RNF10

### 2.4.4 Casos de Teste

| ID Caso de Teste | Requisito Associado | Descrição | Passos de Teste | Resultado Esperado | Prioridade |
|------------------|---------------------|-----------|----------------|-------------------|------------|
| TC_SEC01 | RNF04 | Verificar alerta de dados desatualizados | 1. Simular falha no envio de dados por 5 minutos<br>2. Verificar Dashboard | Alerta “OS DADOS NÃO ESTÃO SENDO ATUALIZADOS” exibido | Essencial |
| TC_SEC02 | RNF08 | Verificar alerta de indisponibilidade do Manager | 1. Desligar serviço do Manager<br>2. Acessar Dashboard | Mensagem de indisponibilidade exibida | Essencial |

# 3. Resultados dos Testes

*Esta seção será preenchida após a execução dos testes, com data, responsável, casos executados, defeitos encontrados e status.*