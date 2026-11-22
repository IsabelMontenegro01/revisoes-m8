# LSTM (Long Short-Term Memory )

&emsp;A LSTM é uma arquitetura de rede recorrente que mantém duas memórias durante a leitura de uma sequência:

- \(c\): memória de longo prazo, também chamada de cell state.
- \(h\): estado oculto, também chamado de hidden state, utilizado como saída da camada e também participa do cálculo das portas seguintes.

&emsp;As duas memórias são atualizadas a cada palavra processada.

&emsp;A LSTM também utiliza a mesma célula e os mesmos pesos em todas as palavras da frase.

&emsp;A principal diferença em relação à SimpleRNN é que a LSTM possui mecanismos chamados portas, que controlam o que deve ser esquecido, armazenado e utilizado.

## 3 Portas da LSTM

&emsp;Três portas para controlar a memória \(c\).

&emsp;Cada porta produz valores entre 0 e 1:
- 0 significa fechada.
- 1 significa aberta.

| Porta | Função |
| :--- | :--- |
| Esquecer | Controla quanto da memória anterior continua armazenado. |
| Entrada | Controla quanto do novo valor será escrito na memória. |
| Saída | Controla quanto da memória $c$ será utilizado para produzir o estado $h$. |

**Porta de esquecer**
- Valor próximo de **0**: apaga a informação.
- Valor próximo de **1**: mantém a informação.

### Atualização das memórias

\[c = esquecer \times c_{anterior}+ entrada \times valor\ novo\]

\[h = saída \times \tanh(c)\]

>As portas não são definidas manualmente. A própria célula calcula seus valores com base na palavra atual e no estado \(h\) anterior.

## O que a LSTM devolve: return_sequences e return_state

&emsp;Assim como na RNN, a LSTM calcula informações durante toda a leitura.

&emsp;Porém, existem duas opções independentes:

- `return_sequences`: controla se a camada entrega o estado \(h\) de cada palavra.
- `return_state`: controla se a camada também entrega os estados finais \(h\) e \(c\).

&emsp;Para manter estados entre lotes, existe outra opção chamada `stateful`.

&emsp;Também existe uma limitação no `Sequential`: quando uma camada entrega múltiplas saídas, a arquitetura precisa permitir que essas saídas sejam conectadas corretamente.

## Montando uma LSTM com Keras

1. **Embedding**
   Transforma IDs das palavras em vetores.

   ↓

2. **LSTM**
   Lê a sequência e atualiza as memórias c e h.

   ↓

3. **Dense**
   Produz a classificação a partir do estado final.


A&emsp; LSTM recebe esses vetores, processa a sequência e mantém suas duas memórias.

&emsp;A `Dense` recebe o estado final \(h\) e produz a classificação.

## Parâmetros da LSTM

&emsp;Na LSTM, existem quatro conjuntos de pesos:

1. Porta de esquecer.
2. Porta de entrada.
3. Porta de saída.
4. Valor novo proposto para a memória.

&emsp;Cada conjunto possui:

- Pesos da entrada.
- Pesos do estado \(h\) anterior.
- Viés.

>O comprimento da frase não altera a quantidade de parâmetros da LSTM, pois os mesmos pesos são reutilizados em todas as posições da sequência.

### Outros fatores na LSTM

#### Dropout

&emsp;O dropout desliga aleatoriamente parte da entrada da LSTM durante o treinamento.

&emsp;O objetivo é alterar o comportamento de aprendizagem da rede.

#### Funções de ativação

&emsp;Duas opções:

- `activation`: função aplicada ao estado \(h\) e ao valor novo proposto. O padrão é tanh.
- `recurrent_activation`: função utilizada pelas portas. O padrão é sigmoid.

### Comparação SimpleRNN x LSTM

&emsp;A SimpleRNN atualiza um único estado a cada palavra, combinando a entrada atual com o estado anterior.

&emsp;A LSTM mantém uma memória \(c\) e um estado \(h\), utilizando três portas para controlar o que é esquecido, armazenado e apresentado como saída.