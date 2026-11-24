# Estudo: Redes Neurais Artificiais e Feedforward

## 1. Introdução às redes neurais artificiais

&emsp;Uma rede neural artificial é um modelo computacional inspirado, de maneira simplificada, no funcionamento de redes de neurônios biológicos. Ela é formada por unidades chamadas neurônios artificiais, organizadas em camadas e conectadas por pesos.

&emsp;O objetivo de uma rede neural é aprender padrões a partir de exemplos. Depois de treinada, ela pode utilizar os padrões aprendidos para realizar tarefas como classificação, previsão ou reconhecimento.

&emsp;Exemplos de aplicações:
- identificar objetos em imagens;
- classificar mensagens como spam ou não spam;
- reconhecer números escritos à mão;
- prever valores com base em dados históricos;
- identificar relações em textos.

&emsp;A rede não recebe necessariamente regras explícitas para cada situação. Durante o treinamento, seus parâmetros são ajustados para que suas previsões fiquem cada vez mais próximas dos resultados esperados.

### Ideia central

&emsp;A rede recebe dados de entrada, realiza operações matemáticas ao longo das camadas e produz uma saída. O aprendizado acontece quando os parâmetros da rede são ajustados com base nos resultados obtidos durante o treinamento.

## 2. Estrutura básica de um neurônio artificial

&emsp;Um neurônio artificial recebe uma ou mais entradas, atribui um peso a cada entrada, soma os resultados, adiciona um viés (*bias*) e aplica uma função de ativação.

&emsp;Considere:
- `x`: valor de entrada;
- `w`: peso associado à entrada;
- `b`: viés (*bias*);
- `z`: resultado da soma ponderada;
- `g`: função de ativação;
- `a`: saída do neurônio.

&emsp;Para várias entradas, a soma ponderada é:

\[z = w_1x_1 + w_2x_2 + \cdots + w_nx_n + b\]

&emsp;Em seguida, a função de ativação é aplicada:

\[a = g(z)\]

### O que cada elemento faz?

&emsp;**Entradas (`x`)**: são os dados apresentados ao neurônio. Podem ser características de uma pessoa, pixels de uma imagem, valores históricos etc.

&emsp;**Pesos (`w`)**: indicam a influência que cada entrada exerce no cálculo. Um peso maior em módulo pode representar uma influência maior, dependendo dos demais valores e da função utilizada. Os pesos são parâmetros aprendidos durante o treinamento.

&emsp;**Viés (`b`)**: é um valor adicional à soma ponderada. Ele permite deslocar o resultado da combinação linear, dando mais flexibilidade ao neurônio. Também é aprendido durante o treinamento.

&emsp;**Função de ativação (`g`)**: transforma o resultado da soma ponderada. Ela é importante para introduzir não linearidade e permitir que redes com várias camadas aprendam relações mais complexas.

&emsp;**Saída (`a`)**: é o resultado produzido pelo neurônio e enviado à próxima etapa da rede ou à saída final.

## 3. Funções de ativação

&emsp;A função de ativação recebe o resultado da soma ponderada e determina a saída do neurônio. A escolha da função influencia o comportamento da rede e o tipo de problema para o qual ela é adequada.

### 3.1 Função degrau (Step)

&emsp;A função degrau retorna um valor discreto de acordo com o resultado recebido. Em sua forma mais simples:

\[
g(z) =
\begin{cases}
1, & z \geq 0\\
0, & z < 0
\end{cases}
\]

&emsp;**Comportamento:** transforma o resultado em uma decisão binária: 0 ou 1.

&emsp;**Aplicação:** pode ser utilizada em modelos simples de classificação binária, como no perceptron clássico.

&emsp;**Limitação:** não é adequada ao treinamento de redes profundas por gradiente descendente, pois sua derivada é zero em quase todos os pontos e não fornece um gradiente útil para ajustar os pesos.

### 3.2 ReLU (Rectified Linear Unit)

&emsp;A ReLU é definida por:

\[
g(z) = \max(0,z)
\]

&emsp;Ou seja:
- se `z` for negativo, a saída será 0;
- se `z` for positivo, a saída será o próprio valor de `z`.

&emsp;**Comportamento:** elimina valores negativos e mantém os positivos.

&emsp;**Aplicação:** é muito utilizada nas camadas ocultas de redes neurais profundas, inclusive em redes voltadas ao processamento de imagens.

&emsp;**Vantagens:**
- é simples e eficiente computacionalmente;
- ajuda a reduzir alguns problemas de gradientes muito pequenos em comparação com sigmoid e tanh em determinadas regiões;
- introduz não linearidade.

&emsp;**Limitação: neurônios mortos (*dying ReLU*)**. Se um neurônio permanecer recebendo valores negativos, sua saída será sempre zero e seu gradiente também pode ser zero. Assim, ele pode deixar de atualizar seus pesos e parar de contribuir para o aprendizado.

### 3.3 Sigmoid

&emsp;A função sigmoid é definida por:

\[
g(z) = \frac{1}{1+e^{-z}}
\]

&emsp;Sua saída fica entre 0 e 1.

&emsp;**Comportamento:**
- entradas muito negativas produzem valores próximos de 0;
- entrada igual a 0 produz saída 0,5;
- entradas muito positivas produzem valores próximos de 1.

&emsp;**Aplicação:** pode ser utilizada na camada de saída de problemas de classificação binária, quando se deseja representar uma probabilidade para uma das classes.

&emsp;**Vantagem:** sua saída entre 0 e 1 facilita a interpretação como probabilidade, quando utilizada com a configuração adequada de treinamento.

&emsp;**Limitação: desaparecimento do gradiente (*vanishing gradient*)**. Para valores muito positivos ou muito negativos, a função fica saturada e sua derivada se aproxima de zero. Isso pode dificultar a atualização dos pesos das camadas mais distantes da saída.

### 3.4 Tanh

&emsp;A função tangente hiperbólica, ou tanh, é definida por:

\[
g(z) = \tanh(z)
\]

&emsp;Sua saída fica entre -1 e 1.

&emsp;**Comportamento:**
- entradas negativas produzem saídas negativas;
- entrada igual a 0 produz saída 0;
- entradas positivas produzem saídas positivas.

&emsp;**Aplicação:** pode ser utilizada em camadas ocultas e aparece em arquiteturas recorrentes, como RNNs e LSTMs, para representar estados com valores positivos e negativos.

&emsp;**Diferença em relação à sigmoid:** a tanh é centrada em zero, enquanto a sigmoid produz valores entre 0 e 1. Isso pode favorecer o comportamento do treinamento em algumas situações.

&emsp;**Limitação:** também pode sofrer com desaparecimento do gradiente quando fica saturada em valores próximos de -1 ou 1.

### 3.5 Softmax

&emsp;A função softmax transforma um conjunto de valores em uma distribuição de probabilidades, em que cada saída fica entre 0 e 1 e a soma de todas as saídas é igual a 1.

&emsp;Para a classe `i`:

\[
softmax(z_i) = \frac{e^{z_i}}{\sum_j e^{z_j}}
\]

&emsp;**Aplicação:** é muito utilizada na camada de saída de problemas de classificação multiclasse com classes mutuamente exclusivas. Por exemplo, classificar uma imagem como gato, cachorro ou pássaro.

&emsp;A softmax permite comparar as classes: a classe com maior probabilidade é a previsão do modelo.

### 3.6 Comparação das funções

| Função | Intervalo de saída | Uso comum |
|---|---:|---|
| Degrau | 0 ou 1 | Perceptron clássico e decisões simples |
| ReLU | 0 até infinito | Camadas ocultas de redes profundas |
| Sigmoid | 0 a 1 | Saída de classificação binária |
| Tanh | -1 a 1 | Camadas ocultas e redes recorrentes |
| Softmax | 0 a 1 por classe; soma 1 | Saída de classificação multiclasse exclusiva |

### Como escolher?

&emsp;A escolha depende da função da camada e do problema:

- **Camadas ocultas:** ReLU é uma escolha comum em redes feedforward profundas.

- **Saída binária:** sigmoid é comum quando se deseja uma probabilidade para a classe positiva.

- **Saída multiclasse exclusiva:** softmax é comum para distribuir a probabilidade entre as classes.

- **Estados com valores positivos e negativos:** tanh pode ser adequada em determinadas arquiteturas.

- **Perceptron clássico:** pode utilizar a função degrau.

&emsp;Não existe uma função universalmente melhor. A escolha depende da arquitetura, dos dados e do objetivo.

### Sigmoid versus Softmax

&emsp;As duas produzem valores entre 0 e 1, mas têm comportamentos diferentes.

- **Sigmoid:** cada saída é calculada independentemente. É apropriada, por exemplo, quando várias classes podem estar presentes ao mesmo tempo ou em classificação binária.

- **Softmax:** as saídas são normalizadas em conjunto e somam 1. É apropriada quando as classes são mutuamente exclusivas e o modelo deve escolher uma delas.

&emsp;Exemplo: em uma imagem que pode conter simultaneamente um gato e um cachorro, sigmoid pode ser usada para estimar cada presença separadamente. Se a imagem deve pertencer a apenas uma classe entre gato, cachorro e pássaro, softmax é uma opção comum.

## 4. Estruturas de redes neurais

&emsp;Uma rede neural pode ser formada por diferentes camadas.

### 4.1 Camada de entrada

&emsp;Recebe os dados que serão processados pela rede. Cada unidade de entrada representa uma característica ou um valor do dado.

&emsp;Exemplo: em uma imagem de 28 × 28 pixels em escala de cinza, pode-se representar a imagem por 784 valores de entrada, um para cada pixel.

### 4.2 Camadas ocultas

&emsp;Ficam entre a entrada e a saída. Elas realizam transformações sucessivas nos dados e permitem que a rede aprenda padrões e relações mais complexas.

&emsp;Uma rede pode ter uma ou várias camadas ocultas.

### 4.3 Camada de saída

&emsp;Produz o resultado final da rede. Sua estrutura e sua função de ativação dependem da tarefa.

&emsp;Exemplos:
- um neurônio com sigmoid para uma classificação binária;
- vários neurônios com softmax para uma classificação multiclasse;
- um ou mais neurônios com saída contínua para determinados problemas de regressão.

### Perceptron e MLP

&emsp;**Perceptron de camada única:** modelo simples que combina entradas, pesos, viés e uma função de ativação. Ele consegue resolver problemas linearmente separáveis.

&emsp;**MLP (Multilayer Perceptron):** rede feedforward com uma camada de entrada, uma ou mais camadas ocultas e uma camada de saída. Com funções de ativação não lineares, pode aprender relações mais complexas.

### Limitação do perceptron simples: XOR

&emsp;O problema XOR (ou exclusivo) tem duas entradas e produz 1 quando as entradas são diferentes e 0 quando são iguais:

| Entrada 1 | Entrada 2 | XOR |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

&emsp;Esses pontos não podem ser separados corretamente por uma única reta. Portanto, o problema não é linearmente separável e um perceptron simples não consegue resolvê-lo.

&emsp;Uma rede com camada oculta e ativações não lineares consegue representar uma fronteira de decisão mais complexa e pode aprender a resolver o XOR.

## 5. Como funciona o Feedforward

&emsp;Feedforward é o processo pelo qual os dados percorrem a rede neural da entrada em direção à saída, passando pelas camadas, sem retornar para camadas anteriores durante essa etapa.

&emsp;O fluxo geral é:

1. A rede recebe os dados de entrada.
2. Cada neurônio calcula uma soma ponderada das entradas.
3. O viés é adicionado.
4. A função de ativação é aplicada.
5. O resultado de uma camada é enviado à próxima.
6. O processo continua até a camada de saída.
7. A rede produz uma previsão.

### Exemplo simplificado

&emsp;Imagine uma rede que recebe duas entradas, `x1` e `x2`, e possui uma camada oculta e uma camada de saída.

- Os valores `x1` e `x2` chegam aos neurônios da camada oculta.
- Cada neurônio combina as entradas utilizando seus próprios pesos e viés.
- A função de ativação transforma o resultado.
- As saídas dos neurônios ocultos tornam-se entradas da camada seguinte.
- A camada de saída calcula o resultado final.

&emsp;O Feedforward é, portanto, o caminho de cálculo da previsão. Ele não é, por si só, o processo de ajuste dos pesos.

## 6. Representação matemática do Feedforward

&emsp;O cálculo de um neurônio pode ser representado por:

\[
z = Wx + b
\]

\[
a = g(z)
\]

&emsp;Em uma camada:
- `x` representa o vetor de entradas;
- `W` representa a matriz de pesos;
- `b` representa o vetor de vieses;
- `g` representa a função de ativação;
- `a` representa o vetor de saídas da camada.

&emsp;De forma resumida:

\[
A_{\text{nova}} = g(WA+B)
\]

&emsp;A saída de uma camada passa a ser a entrada da próxima camada.

&emsp;Para uma rede com várias camadas, o cálculo é repetido sucessivamente. Cada camada possui seus próprios pesos e vieses.

### Por que utilizar matrizes e vetores?

&emsp;A representação matricial permite calcular simultaneamente as operações de vários neurônios, tornando o processamento mais organizado e eficiente, especialmente quando se utilizam bibliotecas e hardware especializados.

## 7. Relação entre Feedforward e aprendizado

&emsp;É importante separar o cálculo da previsão do processo de aprendizado.

&emsp;O **Feedforward** calcula a saída da rede a partir dos dados de entrada e dos parâmetros atuais.

&emsp;O **aprendizado** acontece quando os parâmetros, principalmente pesos e vieses, são ajustados para melhorar as previsões.

&emsp;Em um cenário de aprendizado supervisionado, o processo pode ser descrito assim:

1. A rede recebe uma entrada.
2. O Feedforward produz uma previsão.
3. A previsão é comparada com o valor esperado (*target* ou rótulo).
4. Uma função de perda (*loss function*) calcula o erro.
5. O algoritmo de treinamento utiliza esse erro para calcular como os parâmetros devem ser ajustados.
6. Os pesos e vieses são atualizados.
7. O processo se repete com outros exemplos.

### Quem define que a rede errou?

&emsp;No aprendizado supervisionado, os dados de treinamento fornecem os valores esperados. Esses valores podem ter sido classificados por pessoas ou obtidos de uma fonte de referência.

&emsp;A rede não sabe previamente qual é a resposta correta. Ela produz uma previsão e a função de perda compara essa previsão com a referência disponível.

&emsp;Isso não significa que uma pessoa precise corrigir manualmente cada previsão durante o treinamento. Depois que os dados estão preparados com seus rótulos, o cálculo da perda é automático.

### O que é a função de perda?

&emsp;É uma função que quantifica a diferença entre a previsão do modelo e o valor esperado, de acordo com a tarefa. O valor da perda fornece uma medida do desempenho do modelo naquele exemplo ou conjunto de exemplos.

&emsp;O objetivo do treinamento é ajustar os parâmetros para reduzir a perda.

### O que significa a rede aprender?

&emsp;A rede aprende quando seus pesos e vieses são ajustados de modo que ela passe a produzir previsões melhores para os dados. Ela não aprende como uma pessoa, nem necessariamente armazena regras explícitas. O conhecimento fica representado nos valores dos parâmetros.

## 8. Relação entre Feedforward e Backpropagation

&emsp;Feedforward e Backpropagation são processos diferentes, mas trabalham juntos durante o treinamento de uma rede neural.

### Feedforward

- percorre a rede da entrada até a saída;
- calcula as ativações de cada camada;
- produz a previsão.

### Backpropagation

- utiliza o erro calculado pela função de perda;
- percorre a rede no sentido contrário, da saída em direção às camadas anteriores;
- calcula gradientes que indicam como os pesos e vieses influenciam a perda.

&emsp;Os gradientes são utilizados por um algoritmo de otimização, como o gradiente descendente, para atualizar os parâmetros.

### Fluxo completo do treinamento

1. **Entrada:** os dados são apresentados à rede.
2. **Feedforward:** a rede calcula a previsão.
3. **Cálculo da perda:** a previsão é comparada com o valor esperado.
4. **Backpropagation:** são calculados os gradientes.
5. **Atualização:** o otimizador ajusta pesos e vieses.
6. **Repetição:** o processo continua ao longo dos exemplos e épocas de treinamento.

### Diferença essencial

&emsp;Feedforward responde: “Qual previsão a rede produz com os parâmetros atuais?”

&emsp;Backpropagation responde: “Como cada parâmetro contribuiu para o erro e quais gradientes devem ser utilizados para ajustá-los?”

&emsp;O Backpropagation calcula os gradientes, mas quem efetivamente atualiza os parâmetros é o algoritmo de otimização.

## 9. Conceitos que você precisa dominar

- **Neurônio artificial:** unidade que combina entradas, pesos e viés e aplica uma função de ativação.
- **Entrada:** dado recebido pela rede.
- **Peso:** parâmetro que determina a influência de uma entrada.
- **Viés (*bias*):** parâmetro adicional à soma ponderada.
- **Soma ponderada:** combinação das entradas multiplicadas por seus pesos, acrescida do viés.
- **Função de ativação:** transforma o resultado da soma ponderada e pode introduzir não linearidade.
- **Camada de entrada:** recebe os dados.
- **Camada oculta:** transforma os dados e aprende representações intermediárias.
- **Camada de saída:** produz a previsão.
- **Perceptron:** modelo simples de neurônio artificial.
- **MLP:** rede neural feedforward com múltiplas camadas.
- **Linearmente separável:** situação em que é possível separar classes por uma fronteira linear.
- **Feedforward:** caminho de cálculo da entrada até a saída.
- **Target ou rótulo:** valor esperado usado como referência no aprendizado supervisionado.
- **Função de perda:** calcula uma medida do erro da previsão.
- **Backpropagation:** calcula gradientes para indicar como os parâmetros influenciam a perda.
- **Gradiente descendente:** método de otimização que utiliza gradientes para atualizar os parâmetros.
- **Época (*epoch*):** passagem do algoritmo de treinamento por todo o conjunto de dados de treinamento.
- **Taxa de aprendizado (*learning rate*):** controla o tamanho dos passos utilizados na atualização dos parâmetros.
- **Parâmetros treináveis:** valores, como pesos e vieses, que são ajustados durante o treinamento.

## 10. Resumo geral da aula

&emsp;Uma rede neural artificial é composta por neurônios organizados em camadas. Cada neurônio recebe entradas, multiplica esses valores por pesos, adiciona um viés e aplica uma função de ativação.

&emsp;As funções de ativação determinam como o resultado de cada neurônio será transformado. ReLU é comum em camadas ocultas; sigmoid pode ser utilizada em saídas binárias; softmax é comum em saídas multiclasse exclusivas; tanh produz valores entre -1 e 1; e a função degrau aparece em modelos simples, como o perceptron clássico.

&emsp;O Feedforward é o processo de passagem dos dados da entrada até a saída. Durante esse caminho, cada camada realiza seus cálculos e envia o resultado para a próxima, até produzir uma previsão.

&emsp;No aprendizado supervisionado, a previsão é comparada com um valor esperado. A função de perda calcula o erro. O Backpropagation calcula os gradientes, e o otimizador utiliza esses gradientes para atualizar os pesos e vieses.

&emsp;Assim, Feedforward e Backpropagation têm funções complementares: um calcula a previsão e o outro permite calcular como os parâmetros devem ser ajustados para reduzir o erro.


### Sequência mental para memorizar

&emsp;Essa sequência resume o funcionamento de uma rede neural durante o treinamento.

**Entrada → soma ponderada → viés → função de ativação → próxima camada → previsão → cálculo da perda → Backpropagation → atualização dos parâmetros.**


