## Contexto

1. TF-IDF: uma maneira de calcular a importância de cada palavra em um texto, considerando sua frequência e sua raridade em relação aos outros textos.

2. Word2Vec: uma maneira de transformar palavras em vetores numéricos, de modo que palavras usadas em contextos semelhantes tenham representações matemáticas parecidas.

O material tem quatro páginas HTML:
- [introdução à transformação de texto em vetores](do-texto-aos-vetores.html)
- [atividade interativa de TF-IDF](tfidf-a-mao.html)
- [explicação interativa de Word2Vec](word2vec.html)
- [atividade interativa de Word2Vec com cálculos manuais](word2vec-a-mao.html)

```mermaid
graph TD
    A["Texto original<br><b>&quot;Eu gosto de café&quot;</b>"] --> B["Tokenização<br><b>(Eu)  (gosto)  (de)  (café)</b>"]
    
    B --> C["<b>Representação numérica</b>"]
    
    C --> D["<b>TF-IDF</b><br>Um peso para cada palavra."]
    C --> E["<b>Word2Vec</b><br>Um vetor que representa cada palavra."]
    
    D --> F["<b>O computador consegue fazer cálculos com o texto.</b>"]
    E --> F
    
    %% Estilização opcional para destacar os blocos principais
    style D fill:#0b1320,stroke:#1a2332,color:#fff
    style E fill:#061a12,stroke:#0a291d,color:#fff

```

Antes de estudar as duas técnicas, guarde dois conceitos:
- Token: uma unidade de texto, geralmente uma palavra. Na frase "gosto de café", temos três tokens.
- Corpus: conjunto de textos que estamos analisando. Se tivermos quatro avaliações de uma cafeteria, as quatro juntas formam o corpus.

## 2. Pré-processamento de texto

três textos sobre café:
- Um tweet: "não tomei o cafezinho!!! 😴 #café"
- Um trecho de livro: "O café fumegava na caneca."
- Um artigo científico: "A cafeína do café reduz o sono."

### 2.1 Tokenização
dividir um texto em partes menores chamadas tokens.

Exemplo: 
Eu gosto muito de café -> 
["Eu", "gosto", "muito", "de", "café"]

O [site](do-texto-aos-vetores.html) compara duas formas de fazer isso:

1. Separar pelos espaços

É a forma mais simples. O computador encontra os espaços e divide o texto.

Problema: a pontuação fica grudada nas palavras.

"cafezinho!!!" vira um único token.

2. Usar um tokenizador

O site demonstra a biblioteca NLTK, que possui regras para reconhecer melhor as unidades do texto.

Nesse caso:

"cafezinho!!!" vira:

["cafezinho", "!", "!", "!"]

> Se queremos analisar palavras, geralmente não queremos tratar três pontos de exclamação como parte da palavra.

### 2.2. Vocabulário e identificadores
Depois da tokenização, podemos montar um vocabulário, lista de todas as palavras diferentes encontradas nos textos.

Imagine:
- Texto 1: eu gosto de café
- Texto 2: eu gosto de chá

Vocabulário:
["eu", "gosto", "de", "café", "chá"]

As palavras repetidas aparecem apenas uma vez.

O computador pode atribuir um número para cada palavra:

| Palavra | Identificador |
| :--- | :--- |
| eu | 0 |
| gosto | 1 |
| de | 2 |
| café | 3 |
| chá | 4 |

Assim, o texto 1 pode ser representado por:

[0, 1, 2, 3]

E o texto 2:

[0, 1, 2, 4]

> O número 4 não significa que chá seja maior ou mais importante que café. É apenas uma posição na lista. Não representa o significado da palavra.

Problema: Se procurarmos "cafés", mas só temos "café", o identificador não existe.

### 2.3. Normalização

Padronizar os tokens para evitar que diferenças superficiais sejam tratadas como palavras diferentes.

Exemplos:
- Café → café
- CAFÉ → café
- café! → café (depois da remoção da pontuação)
No site, são demonstradas duas operações:
- Converter tudo para minúsculas (lower()).
- Remover pontuação e símbolos, mantendo letras e números.

Sem normalização, o computador poderia considerar Café, café e CAFÉ três palavras diferentes.

### 2.4. Stop words

Stop words são palavras muito comuns que, em determinadas tarefas, podem não trazer muita informação.

Exemplos em português:
- o
- a
- de
- do
- na
- e

Exemplo:

"Eu gosto de café"

Depois de remover algumas stop words:

"gosto café"

**Por que fazer isso?** palavras como "de" e "o" aparecem tanto que não ajudam a distinguir um texto de outro.

> Remover stop words não é obrigatório,depende da tarefa

o site mostra "não" na lista de stop words usada pelo NLTK, mas ela pode ser extremamente importante e removê-la alteraria completamente o sentido da frase 

exemplo:
"Eu gostei do produto."
"Eu não gostei do produto."

Assim, o stop word é útil quando o foco está no significado central do texto.

### 2.5. Stemming e lematização
Essas duas técnicas tentam reduzir palavras que possuem variações para uma forma comum.

Imagine:
- estudar
- estudando
- estudou
- estudaremos
Todas estão relacionadas ao verbo estudar.

**Stemming**

Ele corta partes da palavra, geralmente sufixos, para encontrar um radical. É um processo mais simples.

Exemplo:

estudando → estud

O resultado nem sempre é uma palavra existente no dicionário.

No site, é utilizado o RSLPStemmer, da biblioteca NLTK para português.

**Lematização**

Tenta encontrar a forma de dicionário da palavra, chamada de lema. É um processo mais linguístico.

Exemplos:
- estudando → estudar
- estudou → estudar
- gatos → gato
O site demonstra essa operação com a biblioteca spaCy.

Diferenças:

| Stemming | Lematização |
| :--- | :--- |
| Corta partes da palavra | Procura a forma-base |
| Mais simples | Mais linguística |
| Pode gerar fragmentos | Busca uma palavra válida |

### 2.6. Subpalavras: BPE (Byte Pair Encoding).

Em vez de considerar cada palavra inteira como uma unidade, podemos dividi-la em pedaços menores.

Imagine a palavra:

cafezinho

Ela poderia ser dividida em:

café + zinho

Ou em outras combinações de caracteres, dependendo do treinamento.

Como o BPE funciona?
1. Começa dividindo as palavras em caracteres.
2. Conta quais pares de caracteres aparecem juntos com maior frequência.
3. Junta o par mais frequente em uma nova unidade.
4. Repete o processo.
Isso ajuda a lidar com palavras que não apareceram exatamente daquela forma durante o treinamento.

Por exemplo, se o modelo conhece café e aprende o sufixo s, pode conseguir representar cafés usando unidades que já conhece.

Essa técnica é muito utilizada em tokenizadores de modelos de linguagem.

## 3. Bag of Words (BoW, ou saco de palavras): transformando textos em tabelas

Agora chegamos à primeira representação numérica que prepara o terreno para TF-IDF.

Representar cada texto contando quantas vezes cada palavra aparece nele.

Imagine dois textos:
- Documento 1: "gosto de café"
- Documento 2: "gosto de chá"
Nosso vocabulário é:

["café", "chá", "de", "gosto"]

Podemos construir uma tabela:

| Documento | café | chá | de | gosto |
| :--- | :--- | :--- | :--- | :--- |
| 1 | 1 | 0 | 1 | 1 |
| 2 | 0 | 1 | 1 | 1 |

Essa tabela é chamada de matriz documento × termo.
- Cada linha representa um documento.
- Cada coluna representa uma palavra.
- Cada célula representa quantas vezes aquela palavra aparece no documento.
Isso também é chamado de TF (Term Frequency) quando estamos considerando a contagem da palavra em cada texto.

> Limitação: BoW não considera a ordem das palavras.

Por exemplo:
- "O cachorro mordeu o homem."
- "O homem mordeu o cachorro."

As duas frases possuem praticamente as mesmas palavras e contagens, mas significados diferentes.

O BoW não consegue perceber essa diferença.

### 3.1. N-gramas: considerando combinações de palavras

Para diminuir essa limitação, o site também demonstra n-gramas.

Um n-grama é uma sequência de N palavras consecutivas.

- Unigrama: uma palavra.
- Bigramas: duas palavras consecutivas.
- Trigramas: três palavras consecutivas.

Exemplo:

"tomei café não chá"

Unigramas:
- tomei
- café
- não
- chá

Bigramas:
- tomei café
- café não
- não chá

O site compara estas frases:
1. "tomei café, não chá"
2. "tomei chá, não café"

Com unigramas, as duas possuem exatamente as mesmas palavras e contagens.

Com bigramas, conseguimos perceber diferenças na ordem e nas combinações.

**Ideia principal:** n-gramas acrescentam um pouco de informação contextual ao Bag of Words, mas ainda não compreendem o significado das frases.

## 4. TF-IDF: descobrindo quais palavras são importantes

Técnica que atribui um peso para cada palavra de cada documento.

A pergunta que ele tenta responder é:

Quais palavras são mais importantes para identificar ou diferenciar este texto dos demais?

Pense em duas situações:
- Uma palavra aparece muitas vezes em um documento.
- Uma palavra aparece em poucos documentos do corpus.

As duas situações são relevantes, mas de maneiras diferentes.

O TF-IDF combina essas duas informações.

### 4.1. TF: Term Frequency (Frequência do termo)

É a quantidade de vezes que uma palavra aparece em determinado documento.

Exemplo:

Documento:

"Eu gosto de café. Café é muito bom."

| Palavra | TF |
| :--- | :--- |
| eu | 1 |
| gosto | 1 |
| de | 1 |
| café | 2 |
| é | 1 |
| muito | 1 |
| bom | 1 |

A palavra café tem TF = 2 porque aparece duas vezes.

Na atividade do professor, o TF é simplesmente a contagem absoluta, sem divisão pelo tamanho do documento.

### 4.2. DF: Document Frequency

Agora imagine que temos quatro documentos.

O DF responde:

Em quantos documentos diferentes essa palavra aparece?

Não importa quantas vezes ela aparece dentro de cada documento. Importa em quantos documentos ela está presente.

Exemplo:

| Documento | café | leite |
| :--- | :--- | :--- |
| 1 | 2 | 1 |
| 2 | 1 | 0 |
| 3 | 3 | 0 |
| 4 | 1 | 0 |

Temos:
- DF(café) = 4, porque café aparece nos quatro documentos.
- DF(leite) = 1, porque leite aparece somente no documento 1.

Perceba a diferença:
- TF conta ocorrências dentro de um documento.
- DF conta documentos que contêm a palavra.

### 4.3. IDF: Inverse Document Frequency (frequência inversa de documentos)

A ideia é dar mais importância às palavras que aparecem em poucos documentos e menos importância às palavras que aparecem em quase todos.

A fórmula utilizada pelo professor é:

$$IDF(t) = \log_2 \left( \frac{N}{DF(t)} \right)$$

Onde:
- \(N\): quantidade total de documentos.
- \(DF(t)\): quantidade de documentos que contêm a palavra.
- \(\log_2\): logaritmo na base 2.
Vamos entender o que acontece.

Imagine que temos quatro documentos.

| DF | N / DF | IDF |
| :--- | :--- | :--- |
| 4 | 4/4 = 1 | 0 |
| 2 | 4/2 = 2 | 1 |
| 1 | 4/1 = 4 | 2 |

Portanto:
- Se uma palavra aparece em todos os documentos, IDF = 0.
- Se aparece em metade dos documentos, IDF = 1.
- Se aparece em apenas um documento, IDF = 2.

**Por que uma palavra comum recebe peso zero?**

Porque ela não ajuda a distinguir um documento dos outros.

Imagine que todos os documentos de uma cafeteria falem sobre café. A palavra "café" aparece em todos eles.

Saber que um documento contém a palavra café não ajuda a identificar qual avaliação estamos lendo.

Já a palavra "canela", que pode aparecer em apenas uma avaliação, ajuda muito mais a diferenciá-la.

### 4.4. TF-IDF: juntando as duas medidas

$$TFIDF(t,d) = TF(t,d) \times IDF(t)$$

- TF mede quantas vezes a palavra aparece naquele documento.
- IDF mede o quanto ela é rara no corpus.
- TF-IDF combina as duas coisas.

Uma palavra terá peso alto quando aparecer bastante no documento e for relativamente rara no conjunto de documentos.
Uma analogia
Imagine que você está tentando identificar qual aluno escreveu uma redação.

Todos os alunos utilizam palavras como "o", "a", "de" e "que". Essas palavras não ajudam muito a identificar o autor.

Mas imagine que apenas um aluno utiliza várias vezes a palavra "astronomia".

Essa palavra é muito mais característica daquela redação.

É justamente essa ideia que o TF-IDF tenta capturar.

>O que você precisa saber fazer em uma prova de TF-IDF
>1. Identificar o vocabulário.
>2. Construir a matriz documento × termo.
>3. Calcular o TF.
>4. Calcular o DF.
>5. Aplicar a fórmula do IDF.
>6. Multiplicar TF × IDF.
>7. Interpretar o significado dos pesos.

## 5. Similaridade de cosseno

Ele mostra como podemos utilizar os vetores TF-IDF para comparar documentos e fazer buscas.

Mas como comparamos dois vetores?

Utilizamos a similaridade de cosseno.

A ideia é medir o ângulo entre dois vetores:
- Cosseno próximo de 1: vetores apontam para direções semelhantes.
- Cosseno próximo de 0: vetores são aproximadamente perpendiculares.
- Cosseno próximo de -1: vetores apontam em direções opostas.

A fórmula é por produto escalar

Isso é uma das ideias por trás dos sistemas de busca textual.

## 6. Word2Vec: ensinando o computador a representar palavras pelo contexto 

O TF-IDF responde:

"Quais palavras são importantes para identificar este documento?"

O Word2Vec tenta responder outra pergunta:

**Como representar uma palavra numericamente de modo que palavras utilizadas em contextos semelhantes tenham vetores semelhantes?**

### 6.1. A ideia da hipótese distribucional

De forma simplificada:
Palavras que aparecem em contextos semelhantes tendem a ter significados ou funções semelhantes.

Vamos imaginar estas frases:
- Eu bebi café quente.
- Eu bebi chá quente.
- Eu bebi leite quente.
Observe:

A palavra café aparece próxima de "bebi" e "quente".

A palavra chá também aparece próxima de "bebi" e "quente".

A palavra leite também poderia aparecer nos mesmos contextos.

Mesmo que o computador não saiba o que é café ou chá, ele consegue perceber que essas palavras são utilizadas de maneira semelhante.

É essa informação que o Word2Vec tenta aprender.

### 6.2. O que é um embedding?

Representação de uma palavra por meio de um vetor numérico.

Os embeddings podem possuir dezenas, centenas ou milhares de dimensões.

E o computador não recebe uma definição pronta de cada palavra. Ele aprende os números durante o treinamento.

### 6.3. Como o Word2Vec aprende?

A ideia é treinar uma rede neural para prever relações entre palavras.

Exemplo:
- bebi café quente
- bebi chá quente
- bebi xurupa quente

Xurupa é uma palavra inventada. Não sabemos o que significa.

Entretanto, ela aparece exatamente no mesmo contexto de café e chá.

A hipótese é que, durante o treinamento, o Word2Vec perceba que xurupa possui um comportamento contextual semelhante ao de café e chá.

Para entender isso, precisamos aprender quatro conceitos.

#### A. Janela de contexto (window)
A janela define quantas palavras ao redor de uma palavra serão consideradas como contexto.

Exemplo:

"bebi café quente"

Com janela 1:
- Contexto de bebi: café.
- Contexto de café: bebi e quente.
- Contexto de quente: café.
Se aumentarmos a janela para 2, poderíamos considerar palavras ainda mais distantes.

Portanto, o tamanho da janela influencia quais relações o modelo aprende.

#### B. Pares de treinamento

No Word2Vec, transformamos as frases em exemplos de treinamento.

Com janela 1, a frase:

"bebi café quente"

Gera:

| Entrada | Contexto |
| :--- | :--- |
| bebi | café |
| café | bebi |
| café | quente |
| quente | café |

Cada par é uma tarefa de aprendizado.

Por exemplo:

café → quente

A rede recebe café e tenta prever quente.

Se ela conseguir prever corretamente os contextos das palavras, estará aprendendo a estrutura de utilização dessas palavras.

#### C. W_in e W_out

Essa é provavelmente a parte mais importante para entender os cálculos do exercício.

O Word2Vec utiliza duas matrizes de pesos:
- \(W_{in}\): pesos utilizados para representar a palavra de entrada.
- \(W_{out}\): pesos utilizados para representar as palavras que podem ser previstas como saída.
Cada palavra possui uma linha em cada matriz.

Imagine que estamos usando apenas duas dimensões:

| Palavra | W_in (x, y) |
| :--- | :--- |
| café | (1, 2) |
| chá | (2, -1) |
| quente | (2, 1) |

Esses valores são os pesos aprendidos pela rede.

Quando recebemos café como entrada, pegamos seu vetor em \(W_{in}\).

Para cada palavra possível de saída, usamos sua linha em \(W_{out}\).

#### D. A rede calcula pontuações

A rede compara o vetor de entrada com cada possível saída usando o produto escalar.

A fórmula é:
\[\text{pontuação}(i,j)=W_{in}(i)\cdot W_{out}(j)\]

Com duas dimensões:
\[s=x_{in}x_{out}+y_{in}y_{out}\]

Quanto maior a pontuação, maior a tendência de a rede prever aquela palavra.

Mas precisamos transformar essas pontuações em probabilidades.

Para isso, usamos a função Softmax:

\[P_j=\frac{e^{s_j}}{\sum_k e^{s_k}}\]

O que ela faz?
1. Recebe as pontuações.
2. Aplica a exponencial em cada uma.
3. Divide cada resultado pela soma das exponenciais.
O resultado é uma distribuição de probabilidades.

Todas as probabilidades somadas precisam dar 1 (ou aproximadamente 1, devido ao arredondamento).

Exemplo:

| Palavra | Probabilidade |
| :--- | :--- |
| bebi | 0,15 |
| café | 0,40 |
| chá | 0,15 |
| quente | 0,15 |
| xurupa | 0,15 |
| Total | 1,00 |

Isso significa que, para aquela entrada, a rede está apostando mais em café.

Mas, se o par de treinamento for café → quente, queremos que a probabilidade de quente aumente.

É aí que entra o erro e a correção dos pesos.

#### Insigths exercícios

época é uma passagem completa pelo conjunto de exemplos de treinamento.

embedding de cada palavra é seu vetor na W_in depois do treinamento.

#### Skip-gram

arquitetura de receber uma palavra e tentar prever as palavras ao redor dela.

Exemplo:

"bebi um café quente"

Se a entrada for café, a rede tenta prever palavras como "um" e "quente", considerando a janela de contexto.

```mermaid
graph TD
    classDef main fill:#0D1117,stroke:#30363D,stroke-width:1px,color:white;
    classDef context fill:#01241C,stroke:#033A2E,stroke-width:1px,color:white;
    
    A["<b>Palavra central</b><br>café"]:::main
    B{"Rede neural"}
    C["<b>Contexto</b><br>um"]:::context
    D["<b>Contexto</b><br>quente"]:::context

    A --> B
    B --> C
    B --> D
```

#### CBOW (Continuous Bag of Words)

Aqui acontece o contrário:

A rede recebe as palavras do contexto e tenta prever a palavra central.

Exemplo:

"bebi um ___ quente"

O modelo recebe o contexto e tenta descobrir que a palavra central é café.

```mermaid
graph TD
    classDef context fill:#0D1117,stroke:#30363D,stroke-width:1px,color:white;
    classDef main fill:#01241C,stroke:#033A2E,stroke-width:1px,color:white;
    
    A["<b>Contexto</b><br>bebi"]:::context
    B["<b>Contexto</b><br>um"]:::context
    C["<b>Contexto</b><br>quente"]:::context
    
    D{"Rede neural"}
    
    E["<b>Palavra prevista</b><br>café"]:::main

    A --> D
    B --> D
    C --> D
    
    D --> E
```

Diferenças: 

| Skip-gram | CBOW |
| :--- | :--- |
| Entrada: palavra central | Entrada: palavras do contexto |
| Saída: palavras vizinhas | Saída: palavra central |
| Palavra → contexto | Contexto → palavra |

Semelhanças: ambos aprendem embeddings a partir das relações entre palavras.

# Comparação: TF-IDF versus Word2Vec

Essa comparação é muito importante porque os dois assuntos têm objetivos relacionados, mas não são a mesma coisa.

| Característica | TF-IDF | Word2Vec |
| :--- | :--- | :--- |
| O que produz? | Pesos para palavras em documentos | Vetores para palavras |
| Como aprende? | Calcula frequências | Treina uma rede neural |
| Considera frequência? | Sim | Indiretamente, pelos exemplos |
| Considera contexto? | Não diretamente | Sim |
| Representa significado? | Não | Captura relações semânticas e sintáticas de maneira aproximada |
| Palavra fora do vocabulário | Não possui representação | Também não possui embedding aprendido |
| Exemplo de uso | Busca de documentos | Similaridade entre palavras |

Vamos imaginar que temos:
- "Eu gosto de café."
- "Eu gosto de chá."

**Com TF-IDF:**
O computador calcula o peso das palavras em cada documento.
Ele pode identificar quais palavras ajudam a diferenciar as frases.

**Com Word2Vec:**
O computador aprende que café e chá aparecem em contextos semelhantes.
Por isso, seus embeddings podem ficar próximos.

**Uma diferença muito importante:**

No TF-IDF, cada documento recebe um vetor de pesos das palavras.
No Word2Vec, cada palavra recebe um vetor aprendido.

É possível combinar as duas técnicas, mas elas não são intercambiáveis.