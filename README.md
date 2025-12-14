# SAFE – Planejamento de Testes com Apoio de Modelos de Linguagem

Este repositório apresenta o **planejamento de testes do sistema SAFE**, desenvolvido como trabalho em equipe no contexto da disciplina. O objetivo foi elaborar artefatos completos de planejamento de testes funcionais e não funcionais, de modo que sua **execução possa ser realizada posteriormente por um time externo e independente de qualidade**, conforme definido na especificação do trabalho.

---

## 1. Contexto do Trabalho

O sistema **SAFE** é um sistema de software **IoT**, composto por múltiplos subsistemas e caracterizado pela interação com dispositivos e fontes de dados que nem sempre se manifestam por meio de interfaces humano-computador.

Após a inspeção e evolução da especificação de requisitos, este trabalho teve como foco:

- Planejar os testes dos requisitos funcionais e não funcionais do sistema SAFE;
- Definir casos de teste e respectivos roteiros;
- Estabelecer critérios de cobertura de testes;
- Maximizar a cobertura de requisitos e funcionalidades com o menor esforço possível;
- Preparar todos os artefatos necessários para execução futura por um time externo de testes.

O uso de **modelos de linguagem de grande porte (LLMs) abertos e gratuitos** foi permitido, desde que os prompts e os modelos utilizados fossem explicitamente documentados, permanecendo sob responsabilidade da equipe a garantia de qualidade, consistência e viabilidade dos artefatos gerados.

---

## 2. Abordagem Adotada

A abordagem adotada combinou práticas tradicionais de **engenharia de testes de software** com o uso controlado e iterativo de LLMs, sempre com validação humana rigorosa.

As principais etapas do processo foram:

1. Conversão dos documentos originais para o formato Markdown;
2. Identificação e separação do sistema SAFE em subsistemas;
3. Definição de prompts estruturados e reutilizáveis;
4. Geração inicial dos planos de teste com apoio de LLMs;
5. Avaliação crítica, refinamento iterativo e correções manuais;
6. Organização dos artefatos finais em um repositório versionado e rastreável.

---

## 3. Estrutura do Repositório

```
.
├── biot
│   ├── plano-de-testes-biot.md
│   ├── prompt-biot.md
│   ├── requisitos-biot.md
│   └── resultado-biot.md
├── dashboard
│   ├── plano-de-testes-dashboard.md
│   ├── prompt-dashboard.md
│   ├── requisitos-dashboard.md
│   └── resultado-dashboard.md
├── geral
│   ├── geral.md
│   ├── persona.md
│   └── template.md
├── manager
│   ├── plano-de-testes-manager.md
│   ├── prompt-manager.md
│   ├── requisitos-manager.md
│   └── resultado-manager.md
└── README.md
```
---

## 4. Documentos Gerais

### 4.1 `geral/geral.md`

Contém informações globais do sistema SAFE, incluindo:
- Documento de visão;
- Subsistemas associados;
- Escopo do sistema;
- Impactos esperados;
- Expectativas;
- Público-alvo.

### 4.2 `geral/persona.md`

Define a **persona utilizada nos prompts enviados às LLMs**, descrevendo:
- O papel a ser assumido pelo modelo;
- A tarefa a ser executada;
- O contexto do sistema SAFE;
- Instruções formais de qualidade, cobertura e formatação dos artefatos esperados.

### 4.3 `geral/template.md`

Apresenta a versão em Markdown do documento **“Template para Plano de Testes – versão 1.0.0”**, utilizado como referência obrigatória para a estrutura dos planos de teste, contemplando:
- Tipos de teste;
- Planejamento e cronograma;
- Responsáveis;
- Casos de teste;
- Roteiros;
- Critérios de entrada e saída.

---

## 5. Subsistemas e Modelos Utilizados

Cada subsistema do SAFE foi tratado de forma independente. Em todos os casos, foram fornecidos às LLMs os seguintes artefatos:

- `geral/geral.md`;
- `geral/persona.md`;
- `geral/template.md`;
- Documento de requisitos específico do subsistema;
- Um rascunho inicial de plano de testes, contendo escopo, objetivos, estratégias, critérios de entrada e saída, bem como um exemplo baseado em um requisito real.

### 5.1 Subsistemas

| Subsistema | Modelo de Linguagem Utilizado |
|-----------|-------------------------------|
| Manager   | ChatGPT                       |
| Dashboard | DeepSeek                      |
| BIoT      | Claude                        |

Para cada subsistema, o repositório contém:
- O **prompt efetivamente utilizado** (`prompt-{subsistema}.md`);
- O **resultado final gerado pela LLM e posteriormente revisado** (`resultado-{subsistema}.md`).

---

## 6. Processo de Avaliação e Refinamento

Os artefatos gerados inicialmente pelas LLMs não foram aceitos automaticamente. O processo de validação envolveu a análise detalhada dos documentos por todos os integrantes do grupo, a solicitação de refinamentos adicionais por meio de prompts subsequentes sempre que necessário, a verificação rigorosa de aderência ao template fornecido, bem como a avaliação da cobertura dos requisitos funcionais e não funcionais. Adicionalmente, foram identificadas e corrigidas manualmente inconsistências, omissões e imprecisões técnicas. Somente após a conclusão desse processo os documentos foram considerados aptos para entrega.

---

## 7. Resultados Obtidos

De maneira geral, os artefatos finais foram considerados adequados, consistentes e alinhados aos objetivos do trabalho, na avaliação dos integrantes do grupo.

Observações sobre os modelos utilizados:

- **ChatGPT (subsistema Manager)**
  Apresentou maior necessidade de refinamento iterativo, demandando ajustes adicionais para atingir o nível de detalhamento e estrutura esperados.

- **Claude (subsistema BIoT)**
  Produziu documentação mais extensa e detalhada, com boa cobertura de requisitos funcionais e não funcionais.

- **DeepSeek (subsistema Dashboard)**
  Gerou artefatos mais concisos, mantendo aderência ao escopo e ao formato solicitado.

---

## 8. Considerações Finais

Este trabalho evidenciou que o uso de LLMs pode apoiar de forma significativa o planejamento de testes de software, desde que:

- Os prompts sejam cuidadosamente elaborados;
- Haja validação humana criteriosa;
- As decisões finais permaneçam sob responsabilidade da equipe.

A organização deste repositório busca garantir transparência, rastreabilidade e reprodutibilidade, atendendo aos requisitos do trabalho e possibilitando a execução futura dos testes por um time externo e independente.
