# Projeto de Automação e Simulação de um Carro de Transferência Usando Factory I/O

Trabalho de Conclusão de Curso — Bacharelado em Engenharia de Controle e Automação
Instituto Federal de Educação, Ciência e Tecnologia de Minas Gerais (IFMG) — Campus Betim

**Autor:** Pedro Henrique da Luz Silva
**Orientadora:** Profa. Deliene Costa Guimarães Barros
**Ano:** 2026

---

## Sobre o projeto

Este trabalho apresenta o desenvolvimento de um **projeto conceitual de um carro de
transferência automatizado com inteligência embarcada**, voltado à automação das etapas de
deslocamento e posicionamento de cargas em ambientes industriais (setores siderúrgico,
cerâmico e refratário).

A solução integra tecnologias consolidadas da automação industrial — controladores lógicos
programáveis, sensores indutivos, sensor de distância a laser, scanners laser de segurança e
comunicação industrial via protocolo **Modbus TCP/IP** — e foi validada por meio da integração
entre o ambiente de engenharia **CODESYS V3.5** e o simulador industrial 3D **Factory I/O**.

O desenvolvimento seguiu o atendimento às normas técnicas **ABNT NBR ISO 12100** (apreciação
e redução de riscos em máquinas) e **IEC 61496-1** (equipamentos de proteção eletrossensíveis).

## Objetivo

Desenvolver um projeto conceitual de um carro de transferência automatizado com inteligência
embarcada, integrando sensoriamento, controle e segurança, para otimizar a movimentação
interna de materiais com maior eficiência operacional e redução de riscos ocupacionais.

**Objetivos específicos:**
- Levantamento de requisitos técnicos e normativos (ABNT NBR ISO 12100, IEC 61496-1)
- Seleção criteriosa de sensores e componentes de controle
- Modelagem e validação da lógica de controle por meio de simulação
- Garantia da segurança operacional via intertravamentos de software
- Documentação técnica completa do projeto

## Tecnologias utilizadas

| Categoria | Tecnologia |
|---|---|
| Ambiente de engenharia / SoftPLC | CODESYS V3.5 |
| Simulação 3D da planta | Factory I/O v2.5.10 |
| Protocolo de comunicação | Modbus TCP/IP (CODESYS como Slave) |
| Sensor de distância a laser | SICK DL100 |
| Scanners de segurança | SICK NanoScan3 |
| Sensores indutivos | Posição, carro carregado, fins de curso da rosca sem fim |
| Linguagem de programação | Ladder (IEC 61131-3) |
| Supervisão / IHM | Telas de visualização integradas do CODESYS |

## Arquitetura da simulação

O CODESYS V3.5 é configurado como **Modbus TCP Slave**, enquanto o Factory I/O atua como
**Modbus TCP/IP Server**, trocando dados continuamente:

- **Entradas digitais**: sinais dos sensores e elementos de supervisão do ambiente simulado
- **Saídas digitais**: acionamento de atuadores e sinalizações, controlados pelo programa Ladder
- **Holding Registers**: valores inteiros como posição de destino (`TargetPosition`) e contadores de ciclo

A validação foi feita com três cenas do Factory I/O integradas em um único ambiente:

1. **Cena 7 — Sistema de armazenamento**: reproduz o comportamento do carro de transferência
   (transelevador), com deslocamento, coleta e depósito de paletes em estrutura de prateleiras.
2. **Cena 16 — Centro de usinagem**: duas máquinas CNC e dois robôs industriais realizando
   carga/descarga automatizada de peças (tampas e bases).
3. **Cena 21 — Separador de matéria-prima**: classificação de itens via sensor de visão,
   direcionando cada produto para uma de três vias classificadoras.

## Lógica de controle (Ladder)

O programa foi estruturado em três rotinas principais:

- **Sistema de Armazenamento** (11 rungs): inicialização, habilitação, seleção de modo
  automático/manual, acionamento de esteiras e garras, contagem de ciclos, envio da posição de
  destino via `TargetPosition`, deposição do palete e retorno ao estado inicial.
- **Centro de Usinagem** (6 rungs): controle independente dos centros de tampas e bases, com
  temporização de saída (15 s) e contadores de produção (`LidsCounter`, `BasesCounter`).
- **Separador de Matéria-Prima** (18 rungs): leitura do sensor de visão, classificação em três
  vias com temporização de 300 ms, contagem individual por via e retorno seguro ao estado inicial.

O sistema de segurança integra os dois scanners NanoScan3 nas extremidades do veículo: qualquer
intrusão detectada interrompe imediatamente o sinal de habilitação do motor (*Enable*),
exigindo rearme manual (*Reset*) após a desobstrução da zona de risco.

## Estratégia de posicionamento

O deslocamento adota uma **janela de transferência** de aproximadamente 1 metro antes da
posição final, dentro da qual a velocidade é reduzida progressivamente — estratégia validada
em múltiplos ciclos consecutivos, sem desvios de posicionamento, *timeout* ou falhas de
intertravamento.

## Resultados

- Posicionamento preciso e repetitivo em múltiplos ciclos consecutivos
- Comunicação Modbus TCP/IP estável entre CODESYS e Factory I/O (estado *Running* contínuo)
- Atuação correta das funções de segurança (parada de emergência via scanners laser)
- Gerenciamento simultâneo de múltiplos atuadores e sensores nas três cenas integradas
- Viabilidade técnica confirmada para aplicação em ambientes industriais reais

## Estrutura do repositório

```
/monografia         → PDF e fontes LaTeX (abntex2) do TCC
/codesys             → Projeto CODESYS V3.5 (.project), lógica Ladder
/factory-io          → Arquivos das cenas 7, 16 e 21 do Factory I/O
/docs                → Diagramas, tabelas de mapeamento I/O, capturas de tela
```

> Ajuste esta seção conforme a organização real das suas pastas.

## Como rodar a simulação

1. Abra o projeto no **CODESYS V3.5** (`/codesys`) e faça o login no controlador virtual (SoftPLC).
2. Abra a cena correspondente no **Factory I/O** (`/factory-io`).
3. No Factory I/O, configure o driver **Modbus TCP/IP Server** com host `192.168.10.40`, porta
   `502` e Slave ID `1`, conforme o mapeamento de I/O do projeto.
4. Inicie a simulação no Factory I/O e coloque o CODESYS em modo *Run*.
5. Acione o botão *Start* (modo automático ou manual) pela interface de supervisão do CODESYS.

## Palavras-chave

Automação industrial; Carro de transferência; Inteligência embarcada; CODESYS; Factory I/O;
Segurança de máquinas; Modbus TCP/IP.

## Autor

**Pedro Henrique da Luz Silva**
Bacharelado em Engenharia de Controle e Automação — IFMG Campus Betim
