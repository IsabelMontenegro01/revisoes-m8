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