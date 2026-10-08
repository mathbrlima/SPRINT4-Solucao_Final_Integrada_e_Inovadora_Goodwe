# SPRINT 4 – Solução Final Integrada e Inovadora Goodwe
# ChargeGrid Intelligence — Gestão Inteligente de Recarga de Veículos Elétricos com Solar e Armazenamento GoodWe

Projeto da Sprint 4 (Solução Final Integrada) — FIAP, Desafio GoodWe.

## Equipe

| Nome | RM |
|------|----|
| Matheus [sobrenome] | [RM] |
| [integrante 2] | [RM] |
| [integrante 3] | [RM] |

## 1. Visão geral

O ChargeGrid Intelligence é uma plataforma de gestão de recarga de veículos elétricos que decide *quando* e *com quanta potência* cada sessão de recarga acontece, a partir do balanço entre geração solar, estado da bateria e consumo do local. O objetivo é maximizar o uso de energia renovável no próprio local, reduzir picos de demanda na rede e diminuir a pegada de carbono da recarga.

## 2. Arquitetura final

```
 Inversor/bateria GoodWe (telemetria simulada: geração, SoC, consumo)
                    |
                    v
 Raspberry Pi Pico (MicroPython) -- calcula saldo = geração - consumo
        LED verde / amarelo / vermelho (estado da sessão de recarga)
                    |
                    v
 Camada de decisão (algoritmo de priorização e preço dinâmico)
                    |
                    v
 Dashboard web (demanda, curvas de potência, preço, alertas, IA)
```

Descrever aqui, em prosa, o fluxo real implementado: de onde vêm os dados, como o Pico os trata, como o dashboard os recebe e qual regra de decisão é aplicada. Inserir o diagrama final em `docs/diagramas/arquitetura.png`.

Regra de decisão do protótipo (ajustar ao que o código realmente faz):

- Saldo positivo (geração acima do consumo): LED verde, recarga liberada com potência plena.
- Saldo próximo de zero: LED amarelo, recarga com potência reduzida.
- Saldo negativo (depende da rede ou da bateria): LED vermelho, sessão pausada ou adiada.

## 3. Alinhamento ao desafio GoodWe e à disciplina

Explicar item a item como cada componente responde a um ponto do desafio:

- **Geração, armazenamento e uso:** o balanço geração − consumo e o estado da bateria definem a política de recarga, usando o modelo de dados de um sistema GoodWe (inversor híbrido e bateria).
- **Gestão de demanda e preço dinâmico:** o dashboard (Sprint 2) mostra a demanda por posto e aplica preço dinâmico para deslocar recargas para horários de maior geração.
- **Interoperabilidade:** o desenho considera OCPP, OCPI e ISO 15118 como camadas de comunicação com carregadores e roaming.
- **Cálculo (Sprint 3):** as curvas de potência dos postos foram modeladas e a energia diária obtida por integral definida.
- **Arquitetura de Computadores (Sprint 3):** o protótipo em Raspberry Pi Pico com MicroPython lê entradas, processa o saldo e aciona atuadores (LEDs).

## 4. Resultados

### 4.1 Energia diária por integral definida

Posto 1 (ChargeGrid, comercial): P1(t) = 5 + 20·sen(πt/24), 0 ≤ t ≤ 24 h.

E1 = ∫₀²⁴ P1(t) dt = 120 + 960/π ≈ 425,6 kWh/dia

Posto 2 (EV ChargeOps, condominial): P2(t) = 16 + 15·cos(π(t − 20)/12), 0 ≤ t ≤ 24 h.

E2 = ∫₀²⁴ P2(t) dt = 384 kWh/dia (o termo cossenoidal integra zero em um período completo)

Esses valores assumem P em kW. O Posto 1 concentra a demanda no meio do dia, coincidindo com a geração solar; o Posto 2 tem pico noturno, o que indica necessidade de armazenamento ou deslocamento de carga.

### 4.2 Resultados do protótipo e do dashboard

Preencher com medições reais ou simuladas do seu sistema, sempre indicando que são simulações quando for o caso:

| Indicador | Sem gestão inteligente | Com ChargeGrid | Ganho |
|-----------|------------------------|----------------|-------|
| Energia solar aproveitada na recarga (%) | [x] | [y] | [y − x] |
| Pico de demanda na rede (kW) | [x] | [y] | [redução] |
| Energia da rede no horário de ponta (kWh) | [x] | [y] | [redução] |
| CO₂ evitado (kg/dia) | — | [z] | — |

Para CO₂ evitado: energia solar usada (kWh) × fator de emissão da rede brasileira (citar a fonte e o ano do fator utilizado).

Inserir prints reais do dashboard em `docs/imagens/` e foto ou captura do circuito do Pico com os três estados dos LEDs.

## 5. Avaliação crítica

Sustentabilidade: uso local de energia renovável, redução de picos e de emissões. Inovação: algoritmo de decisão por saldo energético, preço dinâmico, visualização em tempo real, possibilidade de integração com assistente virtual. Limitações a declarar com honestidade: dados de inversor simulados, ausência de teste com carregador físico, fator de emissão médio e não horário, escala do protótipo. Próximos passos: integração real via API/Modbus do inversor, previsão de geração com modelo de IA, implementação completa de OCPP.

## 6. Tecnologias e referências

Raspberry Pi Pico, MicroPython, Wokwi (ou hardware físico), [linguagem/framework do dashboard], protocolos OCPP, OCPI, ISO 15118, documentação GoodWe (inversores híbridos e baterias), fonte do fator de emissão utilizado. Adicionar links exatos.

## 7. Estrutura do repositório

```
/
├── README.md
├── src/            código do dashboard e da lógica de decisão
├── firmware/       código MicroPython do Raspberry Pi Pico
├── docs/
│   ├── diagramas/
│   └── imagens/
└── data/           dados simulados usados nos resultados
```

## 8. Como executar

Passo a passo para rodar o dashboard e o protótipo (Wokwi ou físico), com dependências e comandos.

## 9. Vídeo

Link do vídeo (YouTube, não listado): [inserir]
