## RNN (Rede Neural Recorrente)

**O que resolve?**

&emsp;Processa sequências e utiliza informações anteriores para interpretar o elemento atual. Cada elemento da sequência é processado considerando o estado oculto anterior, permitindo que a rede mantenha uma memória resumida do que já foi processado.

**Limitação**

&emsp;Tem dificuldade em manter informações por muito tempo devido ao problema do desaparecimento do gradiente (*Vanishing Gradient*).

&emsp;O estado oculto funciona como uma memória resumida das informações processadas até aquele momento.

&emsp;A RNN utiliza os mesmos pesos em todas as etapas da sequência. Isso significa que os parâmetros são compartilhados durante o processamento, independentemente da posição do elemento na sequência.

**Mas, se os pesos são ajustados durante o treinamento, por que não mudam durante o processamento?**

&emsp;Durante o treinamento, os pesos são ajustados para que a rede consiga identificar padrões importantes nos dados e utilizá-los para realizar previsões. Após o treinamento, esses pesos aprendidos são mantidos fixos durante a utilização do modelo (inferência).

&emsp;Portanto, a rede utiliza os mesmos pesos para processar todos os elementos da sequência, mas o estado oculto muda a cada etapa.

**`return_sequences=True`**

&emsp;É utilizado quando queremos retornar a sequência completa de estados ocultos, em vez de apenas o último estado.

&emsp;Por exemplo, para uma sequência com cinco elementos:

- `True`: retorna todos os estados: \(h_1, h_2, h_3, h_4, h_5\).
- `False`: retorna apenas o último estado: \(h_5\).

&emsp;Isso é importante quando queremos alimentar outra camada recorrente com a sequência completa de representações.

**O que significa aprender?**

&emsp;Aprender é ajustar os parâmetros da rede para que ela consiga identificar padrões importantes nos dados e utilizá-los para realizar previsões.

&emsp;Entretanto, acertar o assunto não garante acertar o sentido. No nosso projeto, por exemplo, a IA pode identificar corretamente que o passageiro está falando sobre um banheiro, mas interpretar incorretamente a intenção da solicitação. Isso poderia fazer com que o sistema deixasse de acionar o SSO ou iniciasse uma condução quando não deveria.

---

## LSTM (Long Short-Term Memory)

**O que resolve?**

&emsp;Introduz uma memória de longo prazo e portas que controlam quais informações devem ser armazenadas, esquecidas ou utilizadas. Dessa forma, consegue preservar informações relevantes por mais tempo, reduzindo o problema do desaparecimento do gradiente.

**Limitação**

&emsp;Possui uma estrutura mais complexa, com mais parâmetros, exigindo maior custo computacional e treinamento.

**Duas memórias**

&emsp;A LSTM trabalha com dois estados:

- **Estado da célula (`c`):** representa a memória de longo prazo e transporta informações ao longo da sequência.
- **Estado oculto (`h`):** representa a saída da célula naquele instante e é utilizado no processamento da próxima etapa.

&emsp;O estado `h` também participa do cálculo das portas, ajudando a determinar quais informações serão esquecidas, armazenadas ou utilizadas.

**Três portas**

| Porta | Função |
|---|---|
| Esquecimento (*Forget Gate*) | Controla quanto da memória anterior continuará armazenado. |
| Entrada (*Input Gate*) | Controla quanto das novas informações será escrito na memória. |
| Saída (*Output Gate*) | Controla quanto da memória `c` será utilizado para produzir o estado oculto `h`. |

&emsp;Em resumo:

- **Esquecimento:** define quanto da memória anterior será mantido.
- **Entrada:** define quanto das novas informações será armazenado.
- **Saída:** define quais informações da memória serão utilizadas para produzir o estado oculto.

---

## GRU (Gated Recurrent Unit)

**O que resolve?**

&emsp;Simplifica a estrutura da LSTM, utilizando menos portas para controlar a memória e preservar informações relevantes. Dessa forma, reduz a quantidade de parâmetros e o custo computacional.

**Limitação**

&emsp;Apesar de ser mais simples e eficiente, pode não oferecer o mesmo nível de controle sobre a memória que a LSTM em determinadas tarefas.

**Uma memória e duas portas**

&emsp;Diferentemente da LSTM, a GRU não possui dois estados separados (`c` e `h`). Ela utiliza apenas um estado oculto, que funciona como memória das informações processadas.

**1. Porta de atualização (*Update Gate*)**

&emsp;Controla quanto do estado anterior será mantido e quanto será substituído pelas novas informações.

**2. Porta de reinício (*Reset Gate*)**

&emsp;Controla quanto do estado anterior participará do cálculo das novas informações.

&emsp;Cada porta produz um valor entre 0 e 1 utilizando a função de ativação *Sigmoid*. Esses valores determinam a intensidade com que cada informação será considerada.

---

## Seq2Seq (Sequence to Sequence)

&emsp;O Seq2Seq é um modelo de arquitetura que transforma uma sequência de entrada em outra sequência de saída.

&emsp;Sua estrutura é geralmente organizada em duas partes:

- **Encoder:** lê a sequência de entrada e produz representações das informações processadas.
- **Decoder:** utiliza essas representações para gerar a sequência de saída, um elemento por vez.

&emsp;O Seq2Seq pode utilizar RNNs, LSTMs ou GRUs em sua estrutura.

### Gargalo de informação (Bottleneck)

&emsp;No modelo Seq2Seq tradicional, o Encoder precisa resumir toda a sequência de entrada em um único vetor de contexto, que será transmitido ao Decoder.

&emsp;O problema é que, quando a sequência de entrada é muito longa, informações importantes podem se perder nesse resumo. Isso é conhecido como gargalo de informação (*Information Bottleneck*).

### Attention (Mecanismo de Atenção)

&emsp;O Attention resolve esse problema permitindo que o Decoder consulte diretamente as representações produzidas pelo Encoder a cada etapa da geração.

&emsp;Para isso, o mecanismo calcula pesos que indicam a relevância de cada estado do Encoder naquele momento. Esses pesos são utilizados para produzir uma combinação ponderada dos estados, chamada de vetor de contexto.

&emsp;Assim, a cada palavra gerada, o Decoder consegue direcionar sua atenção às partes mais relevantes da sequência de entrada, em vez de depender exclusivamente de um único resumo de toda a frase.

### Funcionamento do Encoder

**1. Frase em português**

&emsp;A sequência de entrada é transformada em IDs que representam cada palavra.

↓

**2. Embedding**

&emsp;Transforma cada ID em um vetor numérico que representa a palavra.

↓

**3. LSTM**

&emsp;Lê a sequência inteira, palavra por palavra, atualizando seus estados internos a cada etapa.

↓

**4. Estados finais (`h` e `c`)**

&emsp;Representam as informações processadas pelo Encoder e são utilizados para inicializar o Decoder.

### Funcionamento do Decoder

**1. Palavra anterior em inglês**

&emsp;No primeiro passo, recebe o token inicial `<ini>`, que indica o começo da geração da sequência.

↓

**2. Embedding**

&emsp;Transforma o ID da palavra recebida em um vetor numérico.

↓

**3. LSTM**

&emsp;Processa a entrada utilizando os estados `h` e `c` recebidos do Encoder como estados iniciais.

↓

**4. Dense + Softmax**

&emsp;A camada Dense produz pontuações para as palavras do vocabulário inglês, enquanto a Softmax transforma essas pontuações em probabilidades.

↓

**5. Próxima palavra**

&emsp;A palavra escolhida é utilizada como entrada do próximo passo do Decoder, que continua o processo até gerar o token de encerramento da sequência.

---

## Treinamento, Validação e Teste

**Treinamento (*Training*)**

&emsp;Utilizado para ajustar os pesos da rede. O modelo recebe os dados, realiza previsões, calcula os erros e atualiza seus parâmetros para melhorar o desempenho.

**Validação (*Validation*)**

&emsp;Acompanha o desempenho do modelo durante o desenvolvimento e auxilia na escolha de configurações e hiperparâmetros. Também permite identificar problemas como o *overfitting*.

**Teste (*Testing*)**

&emsp;Avalia o desempenho final do modelo utilizando dados separados durante o desenvolvimento. Esses dados não devem ser utilizados para ajustar os parâmetros do modelo.

---

# POSE

## Quaternion e Gimbal Lock

&emsp;O quaternion possui quatro componentes, mas sua norma deve ser igual a 1. Essa restrição faz com que ele tenha apenas três graus de liberdade, assim como as representações de rotação por Euler (Roll, Pitch e Yaw).

&emsp;O *Gimbal Lock* é uma singularidade que ocorre na representação de Euler. Na convenção ZYX, quando o Pitch atinge \(+90°\) ou \(-90°\), os eixos de Roll e Yaw deixam de ser independentes, fazendo com que dois graus de liberdade se sobreponham.

&emsp;A rotação física continua sendo possível. O problema está na representação matemática utilizada para descrever essa rotação.

&emsp;Quaternions e matrizes de rotação evitam essa singularidade porque utilizam outras formas de representar rotações, e não porque possuem graus de liberdade extras.

**Eixos de rotação:**

- **Roll:** rotação em torno do eixo X.
- **Pitch:** rotação em torno do eixo Y.
- **Yaw:** rotação em torno do eixo Z.

&emsp;A associação dos eixos às direções físicas (frente, esquerda e cima/baixo) depende da convenção de coordenadas adotada pelo robô.

## Twist e transformação de coordenadas

&emsp;O Twist representa a velocidade linear e angular do robô em relação a um referencial. No nosso caso, normalmente trabalhamos com o referencial local do robô.

&emsp;Entretanto, para representar o movimento no referencial global do mapa, precisamos transformar essas velocidades utilizando a orientação do robô.

&emsp;A matriz de rotação realiza justamente essa conversão, ajustando as componentes da velocidade linear de acordo com a orientação do robô.

&emsp;Dessa forma, conseguimos transformar uma velocidade descrita no referencial local em uma velocidade correspondente ao referencial global.

---

# MAPAS

## Mapa geométrico

&emsp;Representa as características físicas e espaciais do ambiente, permitindo identificar posições, distâncias, obstáculos e superfícies.

&emsp;Existem diferentes formas de representar geometricamente um ambiente:

**Occupancy Grid (Grade de Ocupação)**

&emsp;Divide o ambiente em pequenas células, classificando cada uma de acordo com a probabilidade de estar livre, ocupada ou desconhecida.

**Landmarks (Marcos de Referência)**

&emsp;São referências reconhecíveis no ambiente, cujas posições podem ser estimadas pelo robô. Podem ser utilizadas para auxiliar na localização e na orientação.

**Nuvem de pontos (*Point Cloud*)**

&emsp;Representa superfícies e objetos do ambiente por meio de um conjunto de pontos em três dimensões, geralmente obtidos por sensores como LiDAR ou câmeras de profundidade.

## Mapa topológico

&emsp;Representa o ambiente por meio de nós e arestas.

&emsp;Os nós representam locais ou pontos de interesse, enquanto as arestas representam conexões navegáveis entre eles.

&emsp;Dessa forma, permite identificar quais caminhos podem ser percorridos para sair de um ponto e chegar a outro, sem necessariamente armazenar todos os detalhes geométricos do ambiente.

## Mapa semântico

&emsp;Acrescenta significado às informações do ambiente, diferenciando regiões que podem ser geometricamente semelhantes, mas possuem funções ou restrições diferentes.

&emsp;Por exemplo, uma área pode estar geometricamente livre, mas ser classificada como restrita. Portanto, estar livre não significa necessariamente que o robô pode acessá-la.

&emsp;No nosso projeto, o mapa semântico pode permitir diferenciar áreas como plataformas, banheiros, bilheterias e regiões restritas.

## Odometria

&emsp;É o processo de estimar o movimento e a pose de um robô ao longo do tempo utilizando medições de sensores e um modelo de movimento.

&emsp;A partir dessas medições, o robô estima quanto se deslocou e como sua posição e orientação foram alteradas.

&emsp;Entretanto, pequenos erros de medição e integração podem se acumular ao longo do tempo, causando deriva (*Drift*) na estimativa da posição.

## Octree e OctoMap

**Octree**

&emsp;É uma estrutura de dados utilizada para organizar informações espaciais em três dimensões. Ela divide um volume em oito regiões menores, que podem ser subdivididas novamente conforme a necessidade.

**OctoMap**

&emsp;É uma representação de mapas tridimensionais que utiliza a estrutura Octree para armazenar informações de ocupação do ambiente.

&emsp;Cada nó da estrutura pode ser dividido em oito partes iguais, permitindo representar diferentes níveis de detalhe espacial.

&emsp;Os volumes são classificados de acordo com sua ocupação:

- **Livre:** região identificada como desocupada.
- **Ocupado:** região identificada como contendo um obstáculo.
- **Desconhecido:** região sobre a qual ainda não existem informações suficientes.

&emsp;Conforme o robô realiza novas medições, as informações são atualizadas, aumentando ou reduzindo a probabilidade de ocupação de cada região.