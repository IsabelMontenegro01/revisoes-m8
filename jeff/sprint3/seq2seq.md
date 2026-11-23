# Seq2Seq (Sequence to Sequence)

&emsp;Arquitetura utilizada quando queremos transformar uma sequência de entrada em outra sequência, que pode ter:
- Quantidade diferente de elementos.
- Ordem diferente.
- Palavras diferentes.
- Relações entre elementos que não são necessariamente posicionais.

&emsp;Exemplo: tradução de reclamações sobre pedidos de café.

1. **Entrada em português**
   o café quente do pedido dois cinco não estava bom

   ↓

2. **Saída em inglês**
   the hot coffee for order two five was not good


&emsp;A tradução não consiste em substituir cada palavra pela correspondente na mesma posição.

&emsp;Existem três particularidades:

1. Inversão da ordem: "café quente" vira "hot coffee".
2. Outra inversão: "não estava" vira "was not".
3. Uma palavra pode virar duas: "gelado" vira "ice cold".

Portanto, não podemos simplesmente utilizar uma rede que recebe uma palavra e produz sua tradução na mesma posição.

&emsp;Precisamos de uma arquitetura capaz de ler a frase inteira e produzir outra sequência, considerando o contexto.

## Como os dados são representados?

&emsp;Assim como nos estudos de RNN e LSTM, as palavras são transformadas em IDs numéricos.

O material possui dois vocabulários:
- PT: vocabulário em português.
- EN: vocabulário em inglês.
Cada palavra recebe um identificador.

&emsp;Além das palavras, existem tokens especiais:

| Token | Significado |
| :--- | :--- |
| `<pad>` | Preenchimento para igualar o tamanho das sequências. |
| `<ini>` | Indica o início da geração do decoder. |
| `<fim>` | Indica que a tradução terminou. |

&emsp;O `<pad>` existe para que frases com compirmentos diferentes sejam processadas em lotes.

# Arquitetura Encoder-Decoder

## O que é o Encoder?

&emsp;O encoder é responsável por ler toda a frase de entrada e transformá-la em uma representação interna.

&emsp;No material, ele utiliza:

1. Embedding.
2. LSTM.

&emsp;O funcionamento é:

1. **Frase em português**
   IDs das palavras

   ↓

2. **Embedding**
   Transforma cada ID em um vetor.

   ↓

3. **LSTM**
   Lê a sequência inteira, palavra por palavra.

   ↓

4. **Estados finais: h e c**
   Representação da frase entregue ao decoder.

## O que é o Decoder?

&emsp;O decoder é responsável por produzir a tradução em inglês, uma palavra por vez.

&emsp;Ele também utiliza:
1. Embedding.
2. LSTM.
3. Dense com softmax.

&emsp;A diferença é que sua LSTM começa com os estados finais do encoder.

&emsp;Além disso, recebe a palavra anterior em inglês.

1. **Palavra anterior em inglês**
   No primeiro passo, recebe `<ini>`.

   ↓

2. **Embedding**
   Transforma o ID em um vetor.

   ↓

3. **LSTM**
   Começa com h e c recebidos do encoder.

   ↓

4. **Dense + Softmax**
   Calcula uma probabilidade para cada palavra do vocabulário inglês.

   ↓

5. **Próxima palavra**
   A palavra escolhida volta como entrada do próximo passo.


&emsp;O encoder entrega seus estados finais:

\[h_{encoder},c_{encoder}\]

&emsp;O decoder recebe esses estados como estado inicial:

\[h_{decoder}^{(0)}=h_{encoder}/\]
\[c_{decoder}^{(0)}=c_{encoder}\]

# Mecanismo de Atenção (Attention)

## Qual problema a atenção resolve?

&emsp;Até aqui, o decoder recebe do encoder somente os estados finais \(h\) e \(c\).

&emsp;Isso significa que o encoder precisa resumir a frase inteira em dois vetores.

&emsp;Imagine uma frase com 14 palavras e uma LSTM com 32 unidades.

&emsp;O encoder entrega:

- 32 números de \(h\).
- 32 números de \(c\).

&emsp;Total: 64 números para representar a frase inteira.

&emsp;Esse é um gargalo: o decoder precisa utilizar essa representação para produzir toda a tradução.

## Como a atenção funciona?

&emsp;A atenção cria outro caminho entre o encoder e o decoder.

&emsp;Em vez de depender somente dos estados finais, o decoder pode consultar os estados do encoder correspondentes a todas as palavras do português.

&emsp;Assim, em cada passo da tradução, o decoder pode buscar as informações mais relevantes para produzir a próxima palavra.

### Sem atenção

1. **Encoder**
   Lê todas as palavras.

   ↓

2. **Somente h e c finais**
   Um resumo da frase inteira.

   ↓

3. **Decoder**


### Com atenção

1. **Encoder**
   Entrega h e c finais e os estados de todas as palavras.

   ↓

2. **Atenção**
   O decoder consulta os estados do encoder em cada passo.

   ↓

3. **Decoder**
   Recebe o contexto relevante para produzir a próxima palavra.

## Resumo comparativo para memorizar

### Encoder x Decoder

| Característica | Encoder | Decoder |
| :--- | :--- | :--- |
| Função | Ler a sequência de entrada | Produzir a sequência de saída |
| Entrada | Português | Palavra anterior em inglês |
| Embedding | Vocabulário português | Vocabulário inglês |
| Camada recorrente | LSTM | LSTM |
| Estado | Produz $h$ e $c$ finais | Começa com $h$ e $c$ do encoder |
| Saída | Representações da entrada | Estados utilizados para prever palavras |

### Seq2Seq sem atenção x com atenção

| Característica | Sem atenção | Com atenção |
| :--- | :--- | :--- |
| Informação do encoder | Estados finais $h$ e $c$ | Estados finais e estados de todas as palavras |
| Consulta às palavras de entrada | Não diretamente | Sim, em cada passo |
| Contexto | Apenas o resumo final | Contexto calculado a cada passo |
| Camada Attention | Não | Sim |
| Entrada da Dense | Estado do decoder | Estado do decoder concatenado ao contexto |

>`Seed` é uma semente utilizada para controlar a geração de números aleatórios. Em redes neurais, ela é frequentemente utilizada para inicializar os pesos de maneira reproduzível.