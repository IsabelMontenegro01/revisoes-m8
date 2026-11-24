# Backpropagation: funcionamento, matemática e aplicação

&emsp;**Ideia central:** o Feedforward calcula a previsão; a função de custo mede o erro; o Backpropagation calcula como cada peso e viés influencia esse custo; e o otimizador utiliza os gradientes para atualizar os parâmetros.

## 1. Por que precisamos do Backpropagation?

&emsp;Em um único neurônio, é relativamente simples entender como uma entrada influencia a saída. Em uma rede multicamada, a saída depende de muitos pesos e vieses, inclusive de camadas anteriores.

&emsp;Precisamos descobrir:

- Quanto cada peso contribuiu para o erro.
- Em que direção cada peso deve mudar.
- Qual deve ser o tamanho relativo dessa mudança.
- Como distribuir a influência do erro entre as camadas.

&emsp;O Backpropagation resolve o problema de calcular a influência de cada parâmetro sobre a função de custo, utilizando derivadas e a regra da cadeia.

>**Backpropagation** significa *retropropagação*: o cálculo da influência do erro começa na saída e segue em direção às camadas anteriores.

## 2. Função de custo: como medir o erro

&emsp;A função de custo, também chamada de função de perda (*loss function*), mede o quanto as previsões da rede estão distantes dos valores esperados.

&emsp;Para um exemplo com uma única saída, utiliza-se o erro quadrático:

$$
C=(a-y)^2
$$

&emsp;Em que:

- $a$: saída prevista pela rede.
- $y$: valor esperado.
- $C$: custo daquele exemplo.

&emsp;Se a previsão estiver próxima do valor esperado, o custo será pequeno. Se estiver distante, o custo será maior.

&emsp;**Exemplo:** se $a=0{,}8$ e $y=1$:

$$
C=(0{,}8-1)^2=0{,}04
$$

&emsp;Se $a=0{,}2$:

$$
C=(0{,}2-1)^2=0{,}64
$$

&emsp;A segunda previsão apresenta um custo maior.

### 2.1 MSE — Mean Squared Error

&emsp;Para vários exemplos, uma forma de calcular o custo médio é o erro quadrático médio, ou MSE:

$$
MSE=\frac{1}{N}\sum_{i=1}^{N}(\hat{y}_i-y_i)^2
$$

&emsp;Para uma saída com vários componentes, como dez classes, pode-se somar os erros quadráticos das saídas:

$$
C=\sum_j(a_j-y_j)^2
$$

&emsp;Depois, calcula-se a média entre os exemplos, conforme a definição adotada.

&emsp;O quadrado faz com que erros positivos e negativos não se cancelem e penaliza mais fortemente erros de maior magnitude.

>A função de custo é a medida que o treinamento busca minimizar. Ela **não** é exatamente a mesma coisa que a taxa de erro de classificação: uma rede pode escolher a classe correta e ainda apresentar perda diferente de zero.

## 3. O custo depende dos pesos e vieses

&emsp;A saída da rede depende dos pesos e vieses. Portanto, o custo também depende deles:

$$
C=f(w_1,w_2,\ldots,w_n,b_1,b_2,\ldots,b_m)
$$

&emsp;Se alterarmos um peso ou viés, a previsão pode mudar e, consequentemente, o custo também pode mudar.

&emsp;A relação pode ser representada por uma curva $C(W)$:

- **Eixo horizontal:** valor do peso $W$.
- **Eixo vertical:** custo $C(W)$.

&emsp;Cada valor do peso produz um custo. O objetivo é encontrar valores dos parâmetros que levem a regiões mais baixas dessa curva.

&emsp;Em uma rede com muitos parâmetros, não existe apenas uma curva em uma dimensão: existe uma **superfície de custo** em muitas dimensões. A rede ilustrativa de reconhecimento de dígitos, com aproximadamente 13 mil parâmetros, possui uma função de custo que depende de aproximadamente 13 mil variáveis.

### 3.1 O que a derivada informa?

&emsp;A derivada mede como uma função varia quando uma variável sofre uma pequena alteração.

&emsp;No contexto da rede, queremos responder:

>Se eu alterar um pouco determinado peso, quanto e em que sentido o custo muda?

&emsp;Isso é representado por:

$$
\frac{\partial C}{\partial w}
$$

- **Derivada positiva:** aumentar o peso tende a aumentar o custo localmente.
- **Derivada negativa:** aumentar o peso tende a diminuir o custo localmente.
- **Derivada próxima de zero:** pequenas mudanças naquele peso têm pouco efeito sobre o custo naquele ponto.

>A derivada descreve o comportamento **local**, ou seja, nas proximidades do valor atual do parâmetro.

### 3.2 Por que subtraímos o gradiente?

&emsp;Imagine uma curva de custo em formato de vale. Se a inclinação é positiva, a curva sobe quando caminhamos para a direita. Para reduzir o custo, devemos caminhar para a esquerda. Se a inclinação é negativa, devemos caminhar para a direita.

&emsp;O gradiente aponta para a direção de maior crescimento local do custo. Para reduzi-lo, caminhamos na direção oposta:

$$
w_{\text{novo}}=w_{\text{atual}}-\eta\frac{\partial C}{\partial w}
$$

&emsp;Esse é o princípio do gradiente descendente (*Gradient Descent*).

&emsp;Em que:

- $w_{\text{novo}}$: novo valor do peso.
- $w_{\text{atual}}$: valor atual do peso.
- $\eta$: taxa de aprendizagem (*learning rate*).
- $\frac{\partial C}{\partial w}$: derivada do custo em relação ao peso.

### 3.3 Exemplo mostrado nos slides

&emsp;Considere:

- Peso atual: $w_5=0{,}6$.
- Taxa de aprendizagem: $\eta=0{,}1$.
- Gradiente: $\frac{\partial C}{\partial w_5}=0{,}2369$.

&emsp;Aplicando a atualização:

$$
w_{5,\text{novo}}=0{,}6-0{,}1\cdot0{,}2369
$$

$$
w_{5,\text{novo}}\approx0{,}5763
$$

&emsp;Como o gradiente é positivo, o peso é reduzido. Isso corresponde a caminhar para uma região de menor custo.

## 4. Regra da cadeia: o coração matemático do Backpropagation

&emsp;Um peso não costuma influenciar o custo diretamente. Ele influencia a soma ponderada de um neurônio; essa soma influencia a ativação; a ativação influencia os neurônios seguintes; e, por fim, a saída influencia o custo.

&emsp;O caminho pode ser representado assim:

$$
w^{(L)}\rightarrow z^{(L)}\rightarrow a^{(L)}\rightarrow C
$$

&emsp;A regra da cadeia permite calcular o efeito total de uma variável sobre outra quando há etapas intermediárias.

>A influência do peso sobre o custo é o **produto** das influências em cada etapa do caminho.

### 4.1 Derivando o custo em relação a um peso de saída

&emsp;Considere:

$$
z^{(L)}=w^{(L)}a^{(L-1)}+b^{(L)}
$$

$$
a^{(L)}=g(z^{(L)})
$$

$$
C=(a^{(L)}-y)^2
$$

&emsp;Queremos calcular:

$$
\frac{\partial C}{\partial w^{(L)}}
$$

&emsp;Pela regra da cadeia, precisamos considerar como o peso influencia a soma ponderada, como a soma ponderada influencia a ativação e como a ativação influencia o custo.

&emsp;A interpretação é:

1. Calculamos a sensibilidade do custo em relação à ativação.
2. Consideramos a derivada da função de ativação.
3. Consideramos a influência do peso sobre a soma ponderada.
4. Multiplicamos essas contribuições para obter a derivada do custo em relação ao peso.

>A ideia principal é que a regra da cadeia permite acompanhar a influência de uma variável ao longo de todas as etapas intermediárias.

### 4.2 Retropropagação para as camadas anteriores

&emsp;Em uma rede multicamada, não basta calcular os gradientes dos pesos da camada de saída. Também precisamos descobrir como os pesos das camadas anteriores influenciam o custo final.

&emsp;Para isso, o Backpropagation utiliza as sensibilidades calculadas na camada seguinte.

&emsp;A interpretação do processo é:

1. Pegamos os sinais de sensibilidade dos neurônios da camada seguinte.
2. Multiplicamos cada um pelo peso da conexão.
3. Somamos as contribuições.
4. Multiplicamos pela derivada da função de ativação do neurônio atual.

&emsp;Assim, a influência do erro é distribuída pelas conexões da rede, permitindo calcular os gradientes dos parâmetros das camadas anteriores.

&emsp;Depois de calcular os gradientes, podemos atualizar os pesos utilizando o gradiente descendente.

&emsp;No exemplo numérico apresentado, também é obtido:

$$
w_{6,\text{novo}}\approx-0{,}9237
$$

&emsp;Para o viés de saída, aplica-se a mesma lógica de atualização:

$$
b_{\text{novo}}=b_{\text{atual}}-\eta\frac{\partial C}{\partial b}
$$

&emsp;A mesma regra é aplicada a todos os pesos e vieses treináveis.

>**Importante:** Backpropagation e gradiente descendente não são a mesma coisa:
>
> - **Backpropagation:** calcula os gradientes.
> - **Gradiente descendente:** utiliza os gradientes para atualizar os parâmetros.

## 5. Treinamento com vários exemplos

### 5.1 Por que não treinar com apenas um exemplo?

&emsp;Se a rede fosse treinada somente com imagens do número 2, poderia ajustar seus parâmetros para responder bem ao número 2, mas não aprenderia adequadamente a reconhecer os outros dígitos.

&emsp;Por isso, a função de custo do treinamento considera muitos exemplos:

$$
J(W,b)=\frac{1}{N}\sum_{i=1}^{N}C_i(W,b)
$$

&emsp;O gradiente total é a média dos gradientes dos exemplos:

$$
\nabla J(W,b)=\frac{1}{N}\sum_{i=1}^{N}\nabla C_i(W,b)
$$

&emsp;Cada exemplo contribui para a direção de atualização dos parâmetros.

### 5.2 Batch, SGD e minilotes

&emsp;Calcular o gradiente utilizando todos os exemplos antes de cada atualização pode ser caro. Há três estratégias comuns:

- **Batch Gradient Descent:** utiliza todo o conjunto de treinamento para calcular o gradiente antes de atualizar os parâmetros.
- **Stochastic Gradient Descent (SGD), em sentido estrito:** atualiza os parâmetros utilizando um exemplo por vez.
- **Mini-batch Gradient Descent:** divide os dados em pequenos lotes e calcula uma aproximação do gradiente com cada minilote. É muito utilizado na prática.

&emsp;**Exemplo:**

- Conjunto de treinamento: 10.000 imagens.
- Tamanho do minilote: 100 imagens.
- Minilotes por época: 100.

&emsp;Para cada minilote:

1. A rede realiza Feedforward.
2. Calcula a perda.
3. Realiza Backpropagation.
4. Calcula os gradientes.
5. Atualiza os parâmetros.

&emsp;A transcrição descreve o SGD como uma caminhada com passos rápidos e um pouco ruidosos em direção a regiões de menor custo. Cada minilote fornece uma estimativa do gradiente que seria obtido com todos os dados.

### 5.3 Época, iteração e minilote

- **Época (*epoch*):** uma passagem completa por todo o conjunto de treinamento.
- **Minilote (*mini-batch*):** subconjunto de exemplos usado para calcular uma atualização.
- **Iteração ou passo:** uma atualização dos parâmetros. Com minilotes, normalmente há uma iteração por minilote.

&emsp;Se existem 1.000 exemplos e o minilote contém 100, uma época possui 10 minilotes e 10 atualizações.

## 6. Dificuldades e cuidados no treinamento

### 6.1 Mínimo local e superfície de custo

&emsp;A função de custo de uma rede neural costuma ser não linear e depender de muitos parâmetros. Sua superfície pode ter vales, regiões planas, pontos de sela e diferentes mínimos locais.

&emsp;O gradiente descendente procura reduzir o custo a partir dos valores atuais. Em geral, não há garantia de encontrar o mínimo global.

&emsp;O resultado depende de fatores como:

- Inicialização dos parâmetros.
- Taxa de aprendizagem.
- Dados utilizados.
- Arquitetura da rede.
- Método de otimização.

### 6.2 Desaparecimento do gradiente (*Vanishing Gradient*)

&emsp;Se os fatores multiplicados durante a retropropagação forem frequentemente pequenos, os gradientes podem ficar cada vez menores ao voltar pelas camadas.

&emsp;Como consequência, os pesos das primeiras camadas recebem atualizações muito pequenas, dificultando seu aprendizado.

&emsp;Isso pode acontecer, por exemplo, quando funções de ativação como *sigmoid* ou *tanh* estão saturadas.

### 6.3 Explosão do gradiente (*Exploding Gradient*)

&emsp;Se os fatores multiplicados forem muito grandes, os gradientes podem crescer excessivamente.

&emsp;As atualizações ficam instáveis e o custo pode oscilar ou divergir.

&emsp;Algumas estratégias que podem ajudar, dependendo da arquitetura:

- Normalização dos dados.
- Inicialização adequada dos pesos.
- Escolha das funções de ativação.
- Normalização de camadas.
- *Clipping* de gradiente.

### 6.4 Generalização e overfitting

&emsp;O objetivo não é apenas reduzir o custo nos exemplos já vistos. A rede também precisa funcionar bem em exemplos novos.

&emsp;Existem três conjuntos importantes:

- **Treinamento:** dados usados para ajustar os parâmetros.
- **Validação:** dados usados para acompanhar o desempenho e orientar escolhas do modelo.
- **Teste:** dados reservados para avaliar o desempenho final em exemplos não usados no ajuste.

&emsp;Se a rede memoriza os exemplos de treinamento e não consegue generalizar, ocorre *overfitting* (sobreajuste).

## 7. Diferenças essenciais

### 7.1 Feedforward versus Backpropagation

| Feedforward | Backpropagation |
|---|---|
| Vai da entrada até a saída. | Parte da saída e segue em direção às camadas anteriores. |
| Calcula as ativações. | Calcula gradientes e sensibilidades. |
| Produz a previsão. | Indica como os parâmetros influenciam o custo. |
| Usa os pesos e vieses atuais. | Calcula derivadas em relação aos pesos e vieses. |
| Não atualiza os parâmetros por si só. | Também não atualiza os parâmetros sozinho; fornece gradientes ao otimizador. |

### 7.2 Backpropagation versus gradiente descendente

| Backpropagation | Gradiente descendente |
|---|---|
| Calcula os gradientes. | É um método de otimização. |
| Usa a regra da cadeia. | Usa os gradientes calculados. |
| Descobre a influência dos parâmetros sobre o custo. | Atualiza os parâmetros na direção oposta ao gradiente. |
| Calcula as derivadas do custo em relação aos pesos e vieses. | Aplica a atualização dos parâmetros. |

&emsp;A atualização de um peso pelo gradiente descendente é:

$$
w\leftarrow w-\eta\frac{\partial C}{\partial w}
$$

&emsp;E a atualização de um viés segue a mesma lógica:

$$
b\leftarrow b-\eta\frac{\partial C}{\partial b}
$$

### 7.3 Erro, custo, gradiente e atualização

- **Erro:** diferença entre previsão e valor esperado, conforme a definição adotada.
- **Custo/perda:** função que quantifica o erro.
- **Gradiente:** conjunto de derivadas parciais do custo em relação aos parâmetros.
- **Backpropagation:** procedimento que calcula os gradientes.
- **Taxa de aprendizagem:** controla o tamanho do passo de atualização.
- **Gradiente descendente:** utiliza os gradientes para atualizar os parâmetros.

## 8. Fluxo completo do treinamento

1. **Preparar os dados:** cada exemplo contém as entradas e, no aprendizado supervisionado, o valor esperado.
2. **Inicializar os parâmetros:** a rede começa com pesos e vieses iniciais.
3. **Feedforward:** os dados percorrem as camadas e a rede produz uma previsão.
4. **Calcular a perda:** a previsão é comparada com o valor esperado.
5. **Backpropagation:** a regra da cadeia calcula os gradientes, começando na saída e voltando pelas camadas.
6. **Atualizar os parâmetros:** o otimizador utiliza os gradientes e a taxa de aprendizagem.
7. **Repetir:** o processo continua com os demais minilotes e épocas.
8. **Avaliar:** verifica-se se a rede aprendeu padrões que também funcionam em dados novos.