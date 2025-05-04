# 🧮 Simulador de Autômatos Finitos

Este repositório contém uma ferramenta desenvolvida em Python para simulação de **autômatos finitos determinísticos (AFD)** e **não determinísticos (AFND)**, com suporte a transições vazias (ε). O projeto foi implementado em um notebook Jupyter (Google Colab) e utiliza arquivos de entrada `.aut` (definição do autômato) e `.in` (palavras a serem testadas).

---

## 📌 Objetivo

Desenvolver um simulador de autômatos finitos capaz de:

- Ler e interpretar a definição de autômatos a partir de arquivos `.aut` no formato JSON.
- Ler palavras de entrada e seus resultados esperados a partir de arquivos `.in`.
- Simular o funcionamento do autômato passo a passo.
- Determinar se cada palavra é **aceita** ou **rejeitada**, comparando com o resultado esperado.
- Medir o tempo de execução da simulação.

---

## ⚙️ Funcionamento

A ferramenta é executada diretamente via Google Colab (notebook `automotos2.ipynb`). A simulação segue os seguintes passos:

1. **Leitura do arquivo `.aut`**:
   - Contém os estados, transições, estado inicial e estados finais do autômato.

2. **Leitura do arquivo `.in`**:
   - Cada linha contém uma palavra e seu resultado esperado, no formato: `palavra;resultado`.

3. **Execução da simulação**:
   - O autômato percorre as transições definidas de acordo com os símbolos da palavra.
   - Para AFNDs, o simulador considera múltiplas possibilidades de caminhos (usando busca).
   - Transições vazias (ε) também são tratadas.
   - Ao final, a palavra é aceita ou rejeitada.

4. **Resultado**:
   - Para cada palavra, é exibido:
     - Palavra testada
     - Resultado esperado
     - Resultado obtido
     - Tempo de execução da simulação

---

## 🧾 Formato dos arquivos

### `.aut` — Definição do autômato

Arquivo em JSON, com os seguintes campos:

```json
{
  "initial": 0,
  "final": [1],
  "transitions": [
    {"from": 0, "read": "a", "to": 1},
    {"from": 1, "read": "b", "to": 1}
  ]
}
