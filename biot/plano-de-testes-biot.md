## Plano de Testes do Sistema BIoT

### 1. Introdução
Este documento descreve o plano de testes para o sistema BIoT, com base no template e nos casos de uso funcionais e não funcionais descritos no documento "requisitos-safe-manager.pdf". O objetivo principal é garantir que todas as funcionalidades sejam implementadas conforme os requisitos, apresentando estabilidade, segurança e usabilidade adequadas.

### 2. Escopo do Teste
O escopo abrange todos os casos de uso definidos e atualizados, incluindo funcionalidades de gerenciamento de instalações, de dispositivos, notificação, solicitações de limpeza, etc.

### 3. Objetivos do Teste
* Validar a conformidade das funcionalidades com os requisitos especificados no questionário atualizado.
* Identificar e reportar defeitos no sistema.
* Garantir a integridade dos dados armazenados, refletindo a estrutura do questionário.
* Verificar a segurança do sistema (autenticação, logs).
* Assegurar a usabilidade e a experiência do usuário, especialmente na navegação.

### 4. Estratégias de Teste
Serão aplicadas as seguintes abordagens de teste:
* **Testes Funcionais**: Verificação se cada função do sistema opera conforme especificado nos casos de uso e campos do questionário atualizado.
* **Testes de Integração**: Confirmação de que os diferentes módulos do sistema interagem corretamente, incluindo a integração de dados, se aplicável.
* **Testes de Regressão**: Reexecução de testes anteriores para garantir que novas alterações não introduziram novos defeitos ou reintroduziram antigos.
* **Testes de Usabilidade**: Avaliação da facilidade de uso e da interface do usuário ao preencher o questionário.
* **Testes de Segurança**: Verificação da robustez da autenticação e do registro de logs.
* **Testes de Dados**: Validação do armazenamento, recuperação e exibição corretos de todos os campos do questionário.

### 5. Ambientes de Teste
* **Hardware**: Computadores e dispositivos móveis.
* **Software**: Sistema operacional compatível (Windows/Linux/macOS), navegadores web (Chrome, Firefox, Edge, Safari), ambiente de desenvolvimento e banco de dados.

### 6. Critérios de Entrada
* Todos os casos de uso devem estar finalizados e revisados.
* Ambiente de teste configurado e disponível.
* Versão do software estável para testes.

### 8. Critérios de Saída
* Todos os casos de teste essenciais aprovados.
* Cobertura de teste mínima de 90% para requisitos essenciais e importantes.
* Número aceitável de defeitos abertos (definido em conjunto com a equipe de desenvolvimento).
* Concordância da equipe sobre a qualidade do produto.

---

## Casos de Teste Detalhados

### RF01 – Obtenção dos limites de temperatura, CO2 e ocupação

* **ID do Caso de Teste**: TC_RF01
    * **Requisito Associado**: RF01 - Obtenção dos limites de temperatura, CO2 e ocupação
    * **Descrição**: Verificar se o dispositivo BIoT obtém corretamente os limites de temperatura, CO2 e ocupação do broker no momento da inicialização.
    * **Pré-condições**: Broker ativo com os parâmetros publicados.
    * **Passos de Teste**:
        1. Ligar o dispositivo BIoT.
        2. Monitorar a comunicação MQTT entre o dispositivo e o broker.
        3. Verificar se os limites recebidos correspondem aos valores publicados no broker.
    * **Resultado Esperado**: O dispositivo BIoT deve obter corretamente os limites de temperatura, CO2 e ocupação do broker no momento da inicialização.
    * **Prioridade**: Essencial