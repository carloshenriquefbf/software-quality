## Plano de Testes do Sistema SAFE Manager

### 1. Introdução
Este documento descreve o plano de testes para o sistema SAFE Manager, com base no template e nos casos de uso funcionais e não funcionais descritos no documento "requisitos-safe-manager.pdf". O objetivo principal é garantir que todas as funcionalidades sejam implementadas conforme os requisitos, apresentando estabilidade, segurança e usabilidade adequadas.

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

### RF01 – Gerenciamento de Gestores das Instalações

* **ID do Caso de Teste**: TC_RF01
    * **Requisito Associado**: RF01 - Gerenciamento de Gestores das Instalações
    * **Descrição**: Verificar o gerenciamento de gestores das instalações pelos administradores.
    * **Pré-condições**: Administrador logado no sistema.
    * **Passos de Teste**:
        1.  Acessar o módulo de instalações.
        2.  Selecionar a opção "Gerenciar gestores".
        3.  Preencher todos os campos necessários.
        4.  Clicar em "Salvar".
    * **Resultado Esperado**: O sistema armazena os dados, exibe mensagem de sucesso e os dados aparecem corretamente na lista de gestores da instalação.
    * **Prioridade**: Essencial
