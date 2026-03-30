# Laboratório 6 — P2: Construindo um Tokenizador BPE e Explorando o WordPiece
**Instituição:** ICEV — Instituto de Ensino Superior  
**Disciplina:** Tópicos em Inteligência Artificial  
**Professor:** Dimmy Magalhães  
**Autor:** Adler Castro Alves 
## Sobre o projeto

Nesse laboratório eu implementei o algoritmo **Byte Pair Encoding (BPE)** do zero em Python e também explorei na prática o tokenizador **WordPiece** usado pelo BERT multilíngue, através da biblioteca Hugging Face.

---

## Como rodar

```bash
pip install transformers
```

Depois é só executar as células do notebook em ordem no Google Colab.

---

## O que foi feito

### Tarefa 1 — `get_stats(vocab)`
Essa função percorre o vocabulário e conta quantas vezes cada par de símbolos aparece lado a lado. A validação pedida no enunciado passou certinho: o par `('e', 's')` retornou contagem 9, que vem das 6 ocorrências em *newest* e 3 em *widest*.

### Tarefa 2 — `merge_vocab(par, v_in)` + Loop de Fusão
Aqui eu criei a função que pega o par mais frequente e funde ele em uma só unidade dentro do vocabulário. O loop roda 5 vezes no total, e a cada rodada imprime qual par foi fundido e como ficou o vocabulário. Dá pra ver claramente tokens morfológicos se formando, como o sufixo `est</w>` surgindo ao longo das iterações.

### Tarefa 3 — WordPiece com BERT Multilingual
Usei o tokenizador `bert-base-multilingual-cased` do Hugging Face pra tokenizar a frase de teste do enunciado e imprimir os tokens gerados.

---

## O que significa o `##` nos tokens do WordPiece

Quando o tokenizador WordPiece quebra uma palavra em partes menores, ele usa o prefixo `##` pra indicar que aquele pedaço é uma **continuação** da palavra anterior — ou seja, não começa uma palavra nova, mas sim segue a que já estava sendo formada. Por exemplo, a palavra *"inconstitucionalmente"* pode virar algo como `['in', '##cons', '##titu', '##cion', '##al', '##mente']`. O `##mente` significa que esse fragmento se gruda no que veio antes.

Isso resolve um problema clássico dos modelos de linguagem: o vocabulário desconhecido. Em vez de o modelo travar diante de uma palavra rara ou nova e jogar um token genérico `[UNK]` que não diz nada, ele consegue decompor essa palavra em partes menores que já existem no vocabulário de treinamento. Assim, mesmo que o modelo nunca tenha visto *"inconstitucionalmente"* antes, ele ainda consegue representar a palavra de forma útil, sem perder o significado. Isso deixa o modelo muito mais robusto com neologismos, termos técnicos e variações morfológicas.

---

## Uso de IA Generativa

A expressão regular utilizada na função `merge_vocab` foi desenvolvida com auxílio do NotebookLM (Google). O trecho em questão é:

```python
padrao = re.compile(r'(^| )' + grande_par + r'( |$)')
nova_palavra = padrao.sub(
    lambda m: m.group(1) + ''.join(par) + m.group(2),
    palavra
)
```

Esse padrão usa grupos de captura `(^| )` e `( |$)` pra garantir que apenas pares isolados por espaço sejam fundidos, evitando substituições erradas no meio de tokens já formados. O código foi revisado e validado manualmente contra os casos de teste do enunciado antes da entrega.

## Anexo 

**Google Colab:**
[https://colab.research.google.com/drive/1pBpHRG0ANdmswHq9PoZVT1wHPzc8uTJd?usp=sharing]
**Referência:**  
* GOODFELLOW, Ian; BENGIO, Yoshua; COURVILLE, Aaron. Deep Learning. [S. l.]: MIT Press, 2016..
 * JURAFSKY, Daniel; MARTIN, James H. Speech and Language Processing: An Introduction to Natural Language Processing, Computational Linguistics, and Speech Recognition with Language Models. 3. ed. draft. [S. l.]: Stanford University/University of Colorado at Boulder, 2026..
 * RASCHKA, Sebastian. Build a Large Language Model (From Scratch). 1. ed. [S. l.]: Manning (MEAP), 2021..
 * UNIVERSIDADE FEDERAL DO PIAUÍ. Estágio Curricular Supervisionado - Fábrica de Software I: normas para o estágio supervisionado. Teresina: UFPI, 2026..
 * VASWANI, Ashish et al. Atenção é tudo o que você precisa. Tradução de Machine Translated by Google. [S. l.]: Google Brain/Google Research, 2017..

---

## Versionamento

```
%cd /content/Lab06-Construindo-token-BPE
!git push https://Adler-zaith14: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@github.com/Adler-zaith14/Lab06-Construindo-token-BPE 
git tag v1.0
```

