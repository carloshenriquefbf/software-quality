# Requisitos do Sistema SAFE Dashboard

## Requisitos Funcionais

**RF01**
O Dashboard deve exibir a temperatura, em graus Celsius (°C), de cada instalação cadastrada que possua algum dispositivo vinculado, sem necessidade de autenticação.

**RF02**
O Dashboard deve mostrar a concentração de CO2 em partes por milhão (ppm) de cada instalação cadastrada que possua algum dispositivo vinculado, sem a necessidade de autenticação.

**RF03**
O Dashboard deve mostrar o número de pessoas presentes em cada instalação cadastrada que possua algum dispositivo vinculado, sem necessidade de autenticação.

**RF04**
O Dashboard deve mostrar a classificação de risco de cada instalação cadastrada, tendo como base os limites dos parâmetros de biossegurança informados, conforme o Guia de Biossegurança [1], sem a necessidade de autenticação.

**RF05**
O Dashboard deve mostrar o estado de utilização ("bloqueado (em manutenção)", "bloqueado (em limpeza)" e "liberado") de cada instalação cadastrada, sem necessidade de autenticação.

**RF06**
O Dashboard deve apresentar a localização da instalação cadastrada, juntamente com as informações da unidade.

**RF07**
O Dashboard deve apresentar, para cada instalação cadastrada, a evolução dos marcadores (temperatura, concentração de CO2, umidade, VOCs, número de pessoas) na última hora, em gráficos lineares temporais, com intervalo de 2 minutos.

**RF08**
O Dashboard deve exibir os valores máximos de cada medida na última hora.

**RF09**
O Dashboard deve exibir o percentual de umidade de cada instalação cadastrada que possua algum dispositivo vinculado, sem necessidade de autenticação.

**RF10**
O Dashboard deve mostrar a unidade de VOCs de cada instalação que possua algum dispositivo vinculado, sem a necessidade de autenticação.

**RF11**
O Dashboard deve alertar o usuário quando o limite máximo de qualquer medição (número de pessoas, VOCs, CO2, umidade e temperatura) definido no SAFE Manager pelo gerente da instalação for atingido.

**RF12**
O Dashboard deve exibir uma legenda no rodapé da página indicando o que as cores e ícones significam, seguindo:
- → Bloqueado;
- → Liberado;
- ⚠ → Limite máximo atingido

---

## Requisitos Não Funcionais

**RNF01**
O Dashboard deve se comunicar com o subsistema SAFE Manager (servidor) por meio das rotas de API do SAFE Manager.

**RNF02**
O Dashboard deve obter os limites dos parâmetros de segurança das instalações cadastradas, utilizando um dispositivo vinculado ao subsistema SAFE Manager. Deve consultar as informações dos parâmetros no SAFE Manager a partir da rota de API com a seguinte estrutura JSON:

```json
{
  "id": "<valor>",
  "name": "<valor>",
  "installation_requests": ["<valor>"],
  "installation_threshold": [
    {
      "temperature": "<valor>",
      "humidity": "<valor>",
      "ppmco2": "<valor>",
      "number_people": "<valor>"
    }
  ]
}
```

**RNF03**
O Dashboard deve obter os dados enviados pelos dispositivos BIoT do subsistema SAFE Manager (servidor).

**RNF04**
O Dashboard deve manter um alerta “OS DADOS NÃO ESTÃO SENDO ATUALIZADOS”, caso não consiga atualizar as informações no `TEMPO_MAXIMO_ANTES_ALERTA_DASHBOARD` (ver glossário).

**RNF05**
O Dashboard deve obter novos dados enviados pelos dispositivos a cada `INTERVALO_ATUALIZAR_DADOS` (ver glossário).

**RNF06**
O Dashboard deve atualizar os limites dos parâmetros de biossegurança a cada `INTERVALO_ATUALIZACAO_LIMITES` (ver glossário).

**RNF07**
O Dashboard deve atualizar os estados de utilização das instalações cadastradas a cada `INTERVALO_ATUALIZACAO_ESTADO` (ver glossário).

**RNF08**
O Dashboard deve informar caso haja indisponibilidade no subsistema SAFE Manager (servidor).

**RNF09**
O Dashboard deve permitir atualizações por meio de Docker.

**RNF10**
O Dashboard deve oferecer aos Administradores do Sistema um meio direto de acesso ao servidor do Manager em caso de falha sistêmica.

**RNF11**
O Dashboard deve realizar atualizações mensais dos componentes de software.

**RNF12**
O Dashboard deve garantir compatibilidade com, no mínimo, as versões: Chrome 118, Firefox 115, Edge 118 e Safari 16.

**RNF13**
O Dashboard deve oferecer uma interface responsiva ao usuário considerando os seguintes critérios:

**RNF14**
O Dashboard deve apresentar as instalações cadastradas em espaços individuais na interface (cards)

**RNF15**
O Dashboard deve se comunicar com o subsistema SAFE Manager por meio de internet.