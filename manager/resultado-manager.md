# Plano de Testes – Sistema SAFE Manager

**Versão:** 1.0.0

---

## Histórico de Revisão

| Data       | Versão | Descrição                                   | Autor                      |
|------------|--------|----------------------------------------------|----------------------------|
| 14/12/2025 | 1.0.0  | Criação inicial do Plano de Testes Manager  | Analista de Testes Sênior  |

---

## Sumário

1. Introdução
2. Estratégia de Teste
   - Testes Funcionais
   - Testes Não Funcionais
3. Resultados dos Testes

---

# 1. Introdução

## 1.1 Finalidade

Este documento tem como finalidade definir o **Plano de Testes do subsistema SAFE Manager**, descrevendo estratégias, tipos de testes, critérios, ambientes e casos de teste necessários para validar se o sistema atende corretamente aos requisitos funcionais e não funcionais especificados.

---

## 1.2 Escopo

Este plano contempla exclusivamente o **SAFE Manager**, subsistema central do Sistema SAFE, responsável por:

- Gerenciamento de usuários, instalações e dispositivos BIoT;
- Processamento e armazenamento de dados ambientais;
- Controle de estados das instalações;
- Comunicação com Broker MQTT e SAFE Dashboard;
- Notificações, segurança e controle de acesso.

As funcionalidades do **SAFE BIoT** e do **SAFE Dashboard** não são testadas diretamente, sendo consideradas apenas em cenários de integração, conforme descrito no documento **geral.md**.

---

## 1.3 Definições, Acrônimos e Abreviações

Os termos técnicos, siglas e definições utilizados neste documento seguem o **Glossário do Sistema SAFE**, conforme especificado no documento geral do projeto (**geral.md**).

---

## 1.4 Referências

| Título                        | Versão | Data | Onde pode ser obtido |
|--------------------------------|--------|------|----------------------|
| Visão Geral do Sistema SAFE    | 1.0    | —    | Documento `geral.md` |
| Requisitos SAFE Manager        | 1.0    | —    | Documento `requisitos-manager.md` |
| Plano de Testes de Referência  | 1.0    | —    | Documento `plano-de-testes-manager.md` |
| Template de Plano de Testes    | 1.0.0  | —    | Documento `template.md` |

---

## 1.5 Visão Geral

Este documento está organizado da seguinte forma:

- A **Seção 2** descreve a estratégia de testes, os tipos de testes adotados e seus respectivos casos de teste;
- A **Seção 3** é destinada ao registro e acompanhamento dos resultados da execução dos testes.

---

# 2. Estratégia de Teste

A estratégia adotada combina **testes funcionais**, **testes de integração** e **testes não funcionais**, garantindo cobertura completa dos requisitos do SAFE Manager.

---

## 2.1 Testes Funcionais

### 2.1.1 Prazo para realização

- Executados após a disponibilização de uma versão estável do SAFE Manager;
- Reexecutados a cada nova entrega do sistema (testes de regressão).

---

### 2.1.2 Recursos necessários

#### Recursos Humanos
- 1 Analista de Testes;
- 1 Desenvolvedor para suporte técnico.

#### Software
- Ambiente do SAFE Manager;
- Broker MQTT (RabbitMQ);
- Banco de Dados;
- Navegadores compatíveis: Chrome, Firefox, Edge e Safari.

#### Hardware
- Computador Desktop;
- Dispositivo móvel (tablet ou smartphone).

# 2.1.3 Requisitos a serem testados

Requisitos Funcionais: RF01, RF02, RF03, RF04, RF05, RF08, RF12, RF15, RF17, RF18.
Requisitos Não Funcionais: RNF01, RNF05, RNF09, RNF15, RNF18.

# 2.1.4 Casos de Teste Funcionais

---

### **TC_RF01 – Gerenciamento de Gestores das Instalações**

* **Requisito Associado**: RF01 – Gerenciamento de Gestores das Instalações
* **Descrição**: Verificar se o administrador consegue cadastrar, editar e remover gestores das instalações.
* **Pré-condições**:
  - Usuário autenticado com perfil **Administrador do Sistema**.
* **Passos de Teste**:
  1. Acessar o SAFE Manager;
  2. Navegar até o módulo de instalações;
  3. Selecionar a opção “Gerenciar Gestores”;
  4. Inserir os dados do gestor;
  5. Confirmar a operação.
* **Resultado Esperado**:
  O gestor é cadastrado/atualizado/removido com sucesso e exibido corretamente na lista de gestores da instalação.
* **Prioridade**: Essencial

---

### **TC_RF02 – Gerenciamento de Dispositivos BIoT**

* **Requisito Associado**: RF02 – Gerenciamento de Dispositivos BIoT
* **Descrição**: Verificar o cadastro, edição e exclusão de dispositivos BIoT pelo administrador.
* **Pré-condições**:
  - Administrador autenticado no sistema.
* **Passos de Teste**:
  1. Acessar o módulo de dispositivos;
  2. Selecionar “Cadastrar Dispositivo”;
  3. Informar id, versão do software, porta e data de instalação;
  4. Salvar.
* **Resultado Esperado**:
  O dispositivo BIoT é armazenado corretamente e exibido na listagem de dispositivos.
* **Prioridade**: Essencial

---

### **TC_RF03 – Gerenciamento de Instalações**

* **Requisito Associado**: RF03 – Gerenciamento de Instalações
* **Descrição**: Validar o gerenciamento de instalações respeitando a hierarquia de localização.
* **Pré-condições**:
  - Administrador ou Gestor autenticado;
  - Gestor associado a uma hierarquia válida.
* **Passos de Teste**:
  1. Acessar o módulo de instalações;
  2. Criar ou editar uma instalação;
  3. Informar localização, tipo e dispositivo associado;
  4. Salvar.
* **Resultado Esperado**:
  A instalação é cadastrada com sucesso, respeitando a regra de negócio da hierarquia.
* **Prioridade**: Essencial

---

### **TC_RF04 – Solicitação de Manutenção**

* **Requisito Associado**: RF04 – Solicitação de Manutenção
* **Descrição**: Verificar se usuários autorizados conseguem registrar solicitação de manutenção.
* **Pré-condições**:
  - Usuário autenticado (gestor, staff, limpeza ou manutenção);
  - Instalação cadastrada.
* **Passos de Teste**:
  1. Selecionar uma instalação;
  2. Solicitar manutenção;
  3. Confirmar solicitação.
* **Resultado Esperado**:
  Solicitação registrada com estado “Aguardando atendimento”.
* **Prioridade**: Alta

---

### **TC_RF05 – Notificação de Manutenção**

* **Requisito Associado**: RF05 – Notificação da Equipe de Manutenção
* **Descrição**: Validar o envio de notificação por e-mail à equipe de manutenção.
* **Pré-condições**:
  - Solicitação de manutenção cadastrada.
* **Passos de Teste**:
  1. Registrar uma solicitação de manutenção;
  2. Aguardar processamento do sistema.
* **Resultado Esperado**:
  E-mail de notificação enviado corretamente à equipe de manutenção.
* **Prioridade**: Alta

---

### **TC_RF08 – Solicitação de Limpeza**

* **Requisito Associado**: RF08 – Solicitação de Limpeza
* **Descrição**: Verificar se usuários autorizados conseguem solicitar serviço de limpeza.
* **Pré-condições**:
  - Usuário autenticado;
  - Instalação cadastrada.
* **Passos de Teste**:
  1. Selecionar uma instalação;
  2. Solicitar limpeza;
  3. Confirmar solicitação.
* **Resultado Esperado**:
  Solicitação registrada e exibida no sistema.
* **Prioridade**: Alta

---

### **TC_RF12 – Solicitação de Uso da Instalação**

* **Requisito Associado**: RF12 – Solicitação de Uso da Instalação
* **Descrição**: Validar solicitação de uso de uma instalação pelo staff.
* **Pré-condições**:
  - Staff autenticado;
  - Instalação disponível.
* **Passos de Teste**:
  1. Selecionar instalação;
  2. Informar data e horário;
  3. Enviar solicitação.
* **Resultado Esperado**:
  Solicitação registrada com sucesso no sistema.
* **Prioridade**: Essencial

---

### **TC_RF15 – Atualização do Estado da Instalação**

* **Requisito Associado**: RF15 – Atualização do Estado de Utilização
* **Descrição**: Verificar a atualização automática do estado da instalação.
* **Pré-condições**:
  - Instalação monitorada;
  - Eventos registrados (uso, limpeza ou manutenção).
* **Passos de Teste**:
  1. Gerar evento (ex.: solicitação de limpeza);
  2. Acompanhar estado da instalação.
* **Resultado Esperado**:
  Estado atualizado corretamente conforme evento ocorrido.
* **Prioridade**: Essencial

---

### **TC_RF17 – Login no Sistema**

* **Requisito Associado**: RF17 – Login
* **Descrição**: Validar autenticação de usuários no SAFE Manager.
* **Pré-condições**:
  - Usuário previamente cadastrado.
* **Passos de Teste**:
  1. Informar e-mail e senha;
  2. Confirmar login.
* **Resultado Esperado**:
  Usuário autenticado e direcionado ao sistema.
* **Prioridade**: Essencial

---

### **TC_RF18 – Alteração de Senha**

* **Requisito Associado**: RF18 – Alteração de Senha
* **Descrição**: Validar alteração de senha respeitando regras de segurança.
* **Pré-condições**:
  - Usuário autenticado;
  - Primeiro acesso ou senha expirada.
* **Passos de Teste**:
  1. Acessar opção “Alterar Senha”;
  2. Informar nova senha válida;
  3. Confirmar alteração.
* **Resultado Esperado**:
  Senha alterada com sucesso e validada conforme critérios definidos.
* **Prioridade**: Essencial

---

# 2.2.4 Casos de Teste Não Funcionais

---

### **TC_RNF01 – Comunicação com Broker MQTT**

* **Requisito Associado**: RNF01
* **Descrição**: Validar a comunicação do Manager com o broker MQTT.
* **Pré-condições**:
  - Broker MQTT ativo;
  - Manager em execução.
* **Passos de Teste**:
  1. Publicar mensagem no broker;
  2. Verificar consumo pelo Manager.
* **Resultado Esperado**:
  Mensagem recebida e processada corretamente.
* **Prioridade**: Essencial

---

### **TC_RNF05 – Alerta de Falta de Dados**

* **Requisito Associado**: RNF05
* **Descrição**: Verificar alerta de ausência de dados do dispositivo IoT.
* **Pré-condições**:
  - Instalação com BIoT associado.
* **Passos de Teste**:
  1. Interromper envio de dados do BIoT;
  2. Aguardar tempo configurado.
* **Resultado Esperado**:
  Exibição do alerta “FALTA DE DADOS DA INSTALAÇÃO <ID>”.
* **Prioridade**: Alta

---

### **TC_RNF09 – Disponibilidade do Sistema**

* **Requisito Associado**: RNF09
* **Descrição**: Avaliar disponibilidade mensal do SAFE Manager.
* **Pré-condições**:
  - Sistema em ambiente produtivo ou homologação.
* **Passos de Teste**:
  1. Monitorar uptime mensal;
  2. Registrar indisponibilidades.
* **Resultado Esperado**:
  Disponibilidade mínima de 99,9%.
* **Prioridade**: Alta

---

### **TC_RNF15 – Encerramento Automático de Sessão**

* **Requisito Associado**: RNF15
* **Descrição**: Validar encerramento de sessão por inatividade.
* **Pré-condições**:
  - Usuário autenticado.
* **Passos de Teste**:
  1. Permanecer inativo por TEMPO_MAXIMO_INATIVIDADE;
  2. Tentar realizar ação.
* **Resultado Esperado**:
  Sessão encerrada automaticamente e redirecionamento para login.
* **Prioridade**: Essencial

---

### **TC_RNF18 – Responsividade da Interface**

* **Requisito Associado**: RNF18
* **Descrição**: Validar adaptação da interface aos diferentes tamanhos de tela.
* **Pré-condições**:
  - Acesso ao sistema em diferentes dispositivos.
* **Passos de Teste**:
  1. Acessar o sistema em desktop, tablet e mobile;
  2. Navegar pelos módulos.
* **Resultado Esperado**:
  Interface responsiva e funcional em todos os breakpoints definidos.
* **Prioridade**: Média
