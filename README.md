# 🧮 Simulador de Máquina de Turing

Este projeto implementa um **simulador de Máquina de Turing** em Python (testado no Google Colab).  
Ele recebe uma especificação da máquina em **JSON** e um arquivo de entrada da fita em **TXT**,  
e retorna se a entrada é aceita ou rejeitada, além de salvar a fita final em um arquivo de saída.

---

## 📌 Funcionalidades
- Lê a definição da máquina a partir de um arquivo `.json`;
- Lê a entrada da fita de um arquivo `.in` (ou `.txt`);
- Executa passo a passo a Máquina de Turing;
- Mostra no console:
  - `1` → se aceitou
  - `0` → se rejeitou
- Salva a fita final em um arquivo `.txt`.

---

## 🚀 Como usar no Google Colab

### 1. Carregar o simulador
Abra uma célula no Colab, cole o código do simulador e execute para carregar a função `run_tm`.

### 2. Subir os arquivos
No menu lateral do Colab (ícone 📂), clique em **Upload** e envie:
- `duplo_bal.json` (especificação da MT)
- `duplobal.in` (entrada da fita)

Eles ficarão disponíveis em `/content/`.

### 3. Executar a máquina
Rode o comando:

```python
run_tm("/content/duplo_bal.json", "/content/duplobal.in", "/content/saida.txt")
Imprime 1 (aceita) ou 0 (rejeita).

Gera o arquivo saida.txt com a fita final.

4. Conferir a saída
Para ver o conteúdo da fita final:

python
Copiar código
with open("/content/saida.txt") as f:
    print(f.read())
5. Usar outros arquivos de entrada
Se tiver outros arquivos .in, basta subir no Colab e rodar novamente:

python
Copiar código
run_tm("/content/duplo_bal.json", "/content/duplobal2.in", "/content/saida2.txt")
Confira os arquivos disponíveis com:

bash
Copiar código
!ls /content

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

## 1. Carregar o autômato

```
def carregar_automato(nome_arquivo):
    with open(nome_arquivo, 'r') as f:
        data = json.load(f)
    inicial = data["initial"]
    finais = set(data["final"])
    transicoes = {}
    for t in data["transitions"]:
        origem = t["from"]
        simbolo = t["read"]  # pode ser None (ε-transição)
        destino = t["to"]
        transicoes.setdefault(origem, {}).setdefault(simbolo, []).append(destino)
    return inicial, finais, transicoes
```
Explicação passo a passo:

Lê um arquivo .aut contendo a definição de um autômato (em formato JSON).

Extrai:

inicial: o estado inicial (ex: "q0").

final: a lista de estados finais.

transitions: uma lista com dicionários de transições.

Monta a estrutura de transições no formato:partir de q0.
```
transicoes["q0"]["a"] = ["q1"]
transicoes["q1"][None] = ["q2", "q3"]  # transições com ε (None)
```

## 2. Função epsilon_closure

```
def epsilon_closure(estados, transicoes):
    pilha = list(estados)
    closure = set(estados)
    while pilha:
        estado = pilha.pop()
        for prox in transicoes.get(estado, {}).get(None, []):
            if prox not in closure:
                closure.add(prox)
                pilha.append(prox)
    return closure
```
Explicação passo a passo:

Calcula o ε-fechamento: todos os estados acessíveis a partir de um conjunto inicial sem consumir símbolo nenhum.

A pilha serve para iterar pelos estados novos alcançados via transições None.

Exemplo:

Se estado = "q0" e "q0" tem uma transição ε para "q1", e "q1" para "q2", a função retornará {"q0", "q1", "q2"}.

## 3. Função simular_automato

```
def simular_automato(inicial, finais, transicoes, palavra):
    estados_atuais = epsilon_closure({inicial}, transicoes)
    for simbolo in palavra:
        proximos_estados = set()
        for estado in estados_atuais:
            for destino in transicoes.get(estado, {}).get(simbolo, []):
                proximos_estados.update(epsilon_closure({destino}, transicoes))
        estados_atuais = proximos_estados
    return any(estado in finais for estado in estados_atuais)
```
Explicação passo a passo:

Começa no estado inicial e calcula o ε-fechamento dele.

Para cada letra da palavra:

Verifica quais destinos são alcançáveis a partir dos estados atuais.

Aplica ε-fechamento aos destinos (após consumir o símbolo).

Ao final:

Se algum estado atual é um estado final, a palavra é aceita.

## 4. Função executar_simulador

```
def executar_simulador():
    inicial, finais, transicoes = carregar_automato("ex3_naodeterministico.aut")
    with open("ex3_naodeterministico_input.in", 'r') as f_in, open("ex3_naodeterministico_input.out", 'w') as f_out:
        leitor = csv.reader(f_in, delimiter=';')
        for linha in leitor:
            palavra, esperado = linha[0], int(linha[1])
            inicio = time.time()
            obtido = int(simular_automato(inicial, finais, transicoes, palavra))
            tempo = round(time.time() - inicio, 3)
            f_out.write(f"{palavra};{esperado};{obtido};{tempo}\n")
```
Explicação passo a passo:

Lê o autômato a partir do arquivo .aut.

Abre os arquivos:

.in → contém as palavras e os resultados esperados (palavra;1 ou palavra;0).

.out → receberá o resultado da simulação.

Para cada linha:

Separa a palavra e o resultado esperado.

Roda a simulação (simular_automato).

Mede o tempo.

Escreve a saída com: palavra, esperado, resultado obtido e tempo.




