# BIoT

## Requisitos Funcionais

### RF01
O dispositivo BIoT deve, no momento de sua inicialização, obter do broker, por meio do tópico MQTT definido para os parâmetros de biossegurança, os limites de temperatura (em °C), de CO2 e de ocupação da instalação, publicados pelo SAFE Manager.

### RF02
O dispositivo BIoT deve contabilizar cada movimento de entrada e saída de pessoas na instalação por meio dos sensores HC-SR04.

### RF03
O dispositivo BIoT deve coletar a temperatura (em °C) da instalação utilizando o sensor DHT11 ou AHT21.

### RF04
O dispositivo BIoT deve coletar a medida da concentração de eCO2 (CO2 equivalente, em ppm) da instalação, usando o sensor CJMCU-811 ou o ENS160.

### RF05
O dispositivo BIoT deve apresentar o estado da instalação em relação aos parâmetros monitorados (temperatura, CO2, número de pessoas presentes) e emitir um sinal de risco em um painel sinalizador localizado na instalação.

### RF06
O dispositivo BIoT deve coletar a medida de umidade da instalação utilizando o sensor DHT11 ou AHT21.

### RF07
O dispositivo BIoT deve coletar a medida de VOCs (Compostos orgânicos voláteis) (ppm) da instalação.

---

## Requisitos não funcionais

### RNF1
O dispositivo BIoT deve se comunicar com o broker por meio do protocolo MQTT.

### RNF2
O dispositivo BIoT deve enviar as informações dos sensores para o broker utilizando o formato JSON através dos respectivos tópicos:

**SAFE_IAQ:**
```json
{
    "idBIoT": "<MAC number>",
    "timestamp": "<dd/mm/yyyy HH:mm:ss>",
    "temperatura": "<valor>",
    "umidade": "<valor>",
    "co2": "<valor>",
    "vocs": "<valor>",
    "erro": "<valor>"
}
```
Onde o campo "temperatura" deverá estar em °C, "umidade" em percentual, "CO2" em ppm e "VOCs" em ppb; o "erro" corresponde a uma lista com as flags de erro.

**SAFE_ENTRY_FLOW**:

```json
{
    "idBIoT": "<MAC number>",
    "timestamp": "<dd/mm/yyyy HH:mm:ss>",
    "entry_flow": "<valor>",
    "erro": "<valor>"
}
```
Onde o campo "entry_flow" corresponde a uma entrada ou saída de indivíduo, e o campo "erro" corresponde a uma lista de flags de erro.

**RNF3**
O dispositivo BIoT deve obter os valores atualizados dos parâmetros de biossegurança da instalação a cada INTERVALO_ATUALIZACAO_LIMITES.

**RNF4**
O dispositivo BIoT deve consultar as informações dos parâmetros no broker com o tópico MQTT_TOPICO no formato JSON com a seguinte estrutura:

```json
{
    "idBIoT": "<MAC number>",
    "temperaturaMin": "<valor>",
    "temperaturaMax": "<valor>",
    "umidadeMin": "<valor>",
    "umidadeMax": "<valor>",
    "numeroPessoasMin": "<valor>",
    "numeroPessoasMax": "<valor>"
}
```

Os campos "temperaturaMin" e "temperaturaMax" deverão estar em °C; "numeroPessoasMin" e "numeroPessoasMax" deverão ser números inteiros; "umidadeMin" e "umidadeMax" deverão estar em percentual.

**RNF5**
Os dados (Número de pessoas na instalação, Temp, CO2, umidade) devem ser informados com precisão de pelo menos 90%.

**RNF6**
O dispositivo BIoT, em caso de falta de energia, deve ser capaz de retomar automaticamente suas atividades ao restabelecimento do fornecimento de energia.

**RNF7**
O dispositivo BIoT, em caso de falta de internet, deve ser capaz de se reconectar à internet quando ela voltar a estar disponível.

**RNF8**
O dispositivo BIoT deve sincronizar seu relógio com o horário da Internet sempre que publicar um tópico no broker.

**RNF9**
O dispositivo BIoT deve armazenar os dados mais recentes (CO2, VOCs, umidade relativa e temperatura) em memória local, caso haja falha de conexão. Esses dados devem ser enviados ao broker assim que a conexão for restabelecida.

**RNF10**
O dispositivo BIoT deve publicar no Broker os dados referentes ao módulo de qualidade de ar (DHT11/AHT-21) e (CJMCU811/ENS160) em um intervalo de 15 segundos (INTERVALO_ATUALIZACAO_DADOS_QUALIDADE_AR)

**RNF11**
O dispositivo BIoT deve publicar no Broker os dados referentes ao módulo de contagem de pessoas (HC-SR04) quando detectar um evento de entrada/saída.

**RNF12**
O dispositivo BIoT deverá atualizar os limites dos parâmetros de biossegurança (no formato especificado em COM04) assim que se ligar e a cada INTERVALO_ATUALIZACAO_LIMITES.

**RNF13**
O dispositivo BIoT deve coletar e enviar as informações dos sensores de uma instalação de maneira ininterrupta na semana, seguindo os intervalos de medição colocados nos RNF10 e RNF11.

**RNF14**
O dispositivo BIoT deve ser compatível com a plataforma NodeMCU.

**RNF15**
O dispositivo BIoT deve estabelecer comunicação autenticada com o Broker por meio de chaves.

**RNF16**
O dispositivo BIoT não deve permitir acesso ou login via Wi‑Fi.

**RNF17**
O dispositivo BIoT deve criptografar os dados transmitidos com o CryptoComponent, com o protocolo Speck.

**RNF18**
Os dispositivos devem ser encapsulados em uma estrutura protetora para evitar a exposição dos fios.

**RNF19**
O dispositivo BIoT deve possuir um broker privado.

**RNF20**
O dispositivo BIoT deve se comunicar a partir de uma rede local e sem fio.

**RNF21**
O dispositivo BIoT deve ser alimentado por meio de uma fonte 5V, utilizando a porta micro USB.

**RNF22**
O dispositivo BIoT deve reportar falhas reconhecidas durante o processo de coleta de medidas através da inclusão de etiquetas de erro na mensagem que será enviada ao broker. Os erros são incrementados na mensagem de forma binária, permitindo que o backend identifique todas as falhas por meio de operações bit a bit. A definição para cada erro segue a seguinte estrutura:

Falha na obtenção de data e hora pelo servidor (1);

Falha na coleta da medida de temperatura (2);

Falha na coleta da medida de umidade (4);

Falha na coleta da medida de CO2 (8).

```c
#define ERROR_TIME  (1 << 0)  // 1
#define ERROR_TEMP  (1 << 1)  // 2
#define ERROR_HUMID (1 << 2)  // 4
#define ERROR_CO2   (1 << 3)  // 8
#define ERROR_TVOC  (1 << 4)  // 16
```
