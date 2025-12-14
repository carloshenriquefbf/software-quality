## Projeto do Sistema SAFE Manager

### **Requisitos Funcionais**

**RF01** - O Manager deve permitir que os administradores gerenciem os gestores das instalações.

**RF02** - O Manager deve permitir que administradores gerenciem dispositivos BIoT, caracterizados pelas seguintes informações: id, versão do software, porta, data de instalação.

**RF03** - O Manager deve permitir que administradores e gestores das instalações gerenciem as instalações, caracterizadas pelas seguintes informações: localização do espaço e de suas sublocalizações (salas, laboratórios, auditório). **Regra de negócio:** Um gestor de instalação só pode gerenciar instalações incluídas na sua hierarquia de localização.

**RF04** - O Manager deve permitir que gestores, staff, equipe de manutenção, equipe de limpeza informem que uma instalação cadastrada necessita de manutenção.

**RF05** - O Manager deve notificar por e-mail a equipe de manutenção quando alguma manutenção para uma instalação for cadastrada.

**RF06** - O Manager deve permitir que a equipe de manutenção informe o aceite de uma solicitação de serviço de manutenção para uma instalação cadastrada.

**RF07** - O Manager deve permitir que o gerente da instalação informe que uma solicitação de serviço de manutenção foi concluída.

**RF08** - O Manager deve permitir que gestores das instalações, staff, equipe de manutenção, equipe de limpeza informem que uma instalação cadastrada necessita de serviço de limpeza.

**RF09** - O Manager deve notificar a equipe de limpeza quando algum serviço de limpeza for solicitado para uma instalação cadastrada.

**RF10** - O Manager deve permitir que a equipe de limpeza informe o aceite de uma solicitação de serviço de limpeza para uma instalação cadastrada.

**RF11** - O Manager deve permitir que o gerente de instalação informe que uma solicitação de serviço de limpeza foi concluída.

**RF12** - O Manager deve permitir que o Staff solicite o uso de uma instalação cadastrada em determinada data e horário.

**RF13** - O Manager deve armazenar em banco de dados todos os dados recebidos dos dispositivos BIoT, incluindo número de pessoas na instalação, temperatura, CO2, umidade, VOCs, data, hora, identificador da instalação e do dispositivo.

**RF14** - O Manager deve guardar o histórico de eventos (nome do solicitante, data/hora da solicitação, nome do funcionário que atendeu à solicitação, data/hora do atendimento, data/hora da conclusão e a instalação para a qual o pedido foi realizado).

**RF15** - O Manager deve exibir o estado de utilização de cada instalação monitorada, atualizando-o conforme os eventos percebidos pelo sistema. Sendo estes:
*   **"Não Liberada"**; e um dos motivos:
    *   Aguardando limpeza;
    *   Aguardando manutenção;
    *   Aguardando limpeza e manutenção;
    *   Em uso;
*   **"Liberado"**: disponível para uso.


**RF16** - O Manager deve notificar os gestores das instalações em caso de uso não autorizado de instalações cadastradas (por exemplo, alguém acessa uma sala com alto risco de contaminação sem autorização).

**RF17** - O Manager deve permitir o login no SAFE com os dados de e-mail e de senha do usuário.

**RF18** - O Manager deve permitir que o usuário modifique a senha após o primeiro acesso. As senhas devem seguir os critérios:
*   Maior que 6 caracteres;
*   Possuir números;
*   Possuir letras maiúsculas e minúsculas;
*   Possuir caracteres especiais;

---

### **Requisitos não funcionais**

**RNF01** - O Manager deve se comunicar com o broker pelo protocolo MQTT.

**RNF02** - O Manager deve enviar os limites de biossegurança das instalações cadastradas aos dispositivos BIoT por meio do broker e ao subsistema Dashboard, por meio da API.

**RNF03** - O Manager deve obter os dados enviados pelos dispositivos BIoT por meio do broker.

**RNF04** - O Manager deve enviar o estado de uma instalação ao subsistema Dashboard a cada mudança de estado, por meio do Broker.

**RNF05** - O Manager deve exibir, na tela, aos administradores e gestores das instalações o alerta **“FALTA DE DADOS DA INSTALAÇÃO <ID>”** quando detectar a falta de dados de um dispositivo IoT associado a uma instalação cadastrada.

**RNF06** - O Manager deve exibir aos administradores do sistema e gestores das instalações o alerta **“DADOS DO DISPOSITIVO <ID> EM FORMATO ERRADO”** na tela em caso de recebimento de dados (do Broker) em um formato inválido.

**RNF07** - O Manager deve buscar novos dados transmitidos pelos dispositivos BIoT no broker a cada **INTERVALO_ATUALIZAR_DADOS** (ver glossário).

**RNF08** - O Manager deve buscar novas notificações no broker a cada **INTERVALO_ATUALIZACAO_NOTIFICACOES** (ver glossário).

**RNF09** - O Manager deve garantir disponibilidade mínima de 99,9% ao mês, exceto durante as janelas de manutenção previamente comunicadas.

**RNF10** - O Manager deve permitir atualizações de versão por meio de Docker.

**RNF11** - O Manager deve oferecer aos Administradores do Sistema um meio direto de acesso ao servidor do Manager em caso de falha sistêmica.

**RNF12** - O Manager deve realizar atualizações mensais dos componentes de software (dependências, bibliotecas).

**RNF13** - O Manager deve garantir compatibilidade com, no mínimo, as versões: Chrome 118, Firefox 115, Edge 118 e Safari 16.

**RNF14** - O Manager deve realizar o controle de acesso baseado em papéis (administrador do sistema e gestor da instalação), garantindo que apenas o pessoal autorizado acesse elementos da rede, informações armazenadas, serviços e aplicações.

**RNF15** - O Manager deve encerrar automaticamente a sessão do usuário após atingir o tempo máximo de inatividade configurado (**TEMPO_MAXIMO_INATIVIDADE**), contado a partir da última ação do usuário.

**RNF16** - O Manager deve utilizar o CryptoComponent, com o protocolo Speck, para o tratamento dos dados recebidos dos dispositivos BIoT.

**RNF17** - O Manager deve utilizar algoritmos de criptografia, com o CryptoComponent, usando o protocolo Speck, para o tratamento dos dados enviados aos dispositivos BIoT.

**RNF18** - O Manager deve oferecer uma interface responsiva ao usuário, adaptando o layout e os elementos da interface de acordo com os seguintes pontos de interrupção (*breakpoints*) baseados na largura da viewport:
*   **Grande (Desktop)**: Para telas com **1008px ou mais** de largura (ex.: computadores, laptops, Surface Hubs).
*   **Médio (Tablet)**: Para telas com largura entre **641px e 1007px** (ex.: tablets, phablets).
*   **Pequeno (Mobile/TV)**: Para telas com **640px ou menos** de largura (ex.: smartphones, TVs).

**RNF19** - O Manager deve consumir o que o BIoT publicou via broker nos tópicos (escritos em JSON) **"SAFE_IAQ"** e **"SAFE_ENTRY_FLOW"**.