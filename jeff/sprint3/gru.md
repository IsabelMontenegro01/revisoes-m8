# GRU (Gated Recurrent Unit)

&emsp;Uma arquitetura recorrente que, assim como a LSTM, utiliza portas para controlar o fluxo de informações.

A diferença principal é que a GRU mantém uma única memória, representada por \(h\).

Duas portas controlam como o estado é atualizado.

A GRU procura combinar o controle de memória de uma arquitetura com portas com uma estrutura mais simples que a LSTM.

Entretanto, ter menos componentes não significa necessariamente aprender mais rápido ou apresentar melhores resultados.

## Como a GRU lê uma frase

Assim como as outras redes recorrentes, a GRU lê uma palavra por vez.

A cada palavra:
1. Recebe o vetor da palavra atual.
2. Considera o estado \(h\) anterior.
3. Calcula suas portas.
4. Atualiza o estado \(h\).
5. Passa o novo estado para a próxima palavra.

A mesma célula e os mesmos pesos são utilizados em todas as palavras.

A diferença em relação à SimpleRNN é que a GRU possui duas portas para controlar a atualização da memória.

## As duas portas da GRU

A GRU utiliza:
1. Porta de atualização (update gate).
2. Porta de reinício (reset gate).

Cada porta produz um valor entre 0 e 1, utilizando a função sigmoid.

### Porta de atualização

Controla quanto do estado anterior será mantido e quanto será substituído pelo valor novo.

Segundo a convenção apresentada no material:

- Valor próximo de 1: mantém o estado anterior.
- Valor próximo de 0: utiliza o valor novo.

Essa porta controla simultaneamente a manutenção do passado e a incorporação de novas informações.

### Porta de reinício

Controla quanto do estado anterior participa do cálculo do valor novo.

- Valor próximo de 0: ignora o passado ao calcular o valor novo.
- Valor próximo de 1: utiliza o estado anterior normalmente.

## O que a GRU devolve: `return_sequences` e `return_state`

## Quantidade de parâmetros da GRU

Três conjuntos de pesos:

1. Porta de atualização.
2. Porta de reinício.
3. Valor novo proposto.

## Treinamento da GRU

1. **Embedding**
   Transforma IDs em vetores.

   ↓

2. **GRU**
   Processa as palavras e atualiza h.

   ↓

3. **Dense + Sigmoid**
   Produz a probabilidade da frase ser positiva.

