### Qual problema vocês estão tentando resolver?

&emsp;Dúvidas recorrentes sobre trajetos, baldeações, plataformas, serviços e acessibilidade. Em horários movimentados

&emsp;Atendimento informacional: responde às perguntas dos passageiros por voz e/ou toque, utilizando IA generativa e informações institucionais.

&emsp;Orientação física: quando necessário, desloca-se de forma autônoma até um ponto de interesse, acompanhando o passageiro.

&emsp;Robô deve encaminhar situações críticas à SSO

&emsp;Arquitetura modular - solução desacoplada da plataforma robótica. Assim, as camadas de interface, atendimento e IA não devem depender diretamente de um hardware específico

---

## Grafos

&emsp;Estrutura matemática usada para representar entidades e as conexões entre elas.

&emsp;Vértices representam as entidades, arestas representam as conexões e pesos representam o custo de percorrer essas conexões

&emsp;O robô considera os caminhos disponíveis e os custos associados.

- **Vértice** - Par (estação, linha)

- **Aresta de viagem** - Conexão entre estações consecutivas de uma linha

- **Aresta de baldeação**	- Conexão entre linhas diferentes de uma mesma estação

- **Peso** - Tempo estimado, em minutos

- **Direção** - Alguns trechos podem ter sentidos específicos, como o Expresso Aeroporto

#### Vértices separados:

&emsp;Estar na Luz pela Linha 1 não é exatamente o mesmo estado que estar na Luz pela Linha 4, pois a troca entre essas linhas exige um deslocamento físico e consome tempo.

&emsp;Se ambas fossem representadas por um único vértice, essa diferença poderia desaparecer do modelo

#### Grafo ponderado

&emsp;Nem toda conexão tem o mesmo custo.

&emsp;Se o objetivo é oferecer o trajeto mais rápido, não basta encontrar um caminho com poucas conexões: é necessário comparar a soma dos pesos.

&emsp;O critério padrão é minimizar o tempo estimado, usando o número de baldeações como desempate. Também existe a opção de priorizar menos baldeações, usando o tempo como desempate. (relevante para pessoas com bagagem ou mobilidade reduzida.)

---

## Algoritmo

A* considera dois valores:
- g(v): o custo já percorrido da origem até o vértice atual.
- h(v): uma estimativa do custo que falta até o destino.

### A* versus Dijkstra 

&emsp;A* pode examinar menos vértices quando a heurística é informativa

&emsp;Heurística é uma estimativa de quanto falta para chegar ao destino.

&emsp;O algoritmo prioriza os vértices com menor valor estimado de custo total.

&emsp;Heurística informativa é quando ajuda a distinguir quais caminhos parecem levar mais diretamente ao destino

&emsp;Heurística do A* usa a distância geográfica em linha reta até o destino, dividida por uma velocidade máxima estimada da rede

&emsp;A documentação da busca de rotas registra 226 vértices e 539 arestas direcionadas. Uma matriz teria 226^2=51.076 posições, embora aproximadamente 1% delas corresponda a conexões existentes. Por isso, o grupo utiliza lista de adjacência (Cada vértice armazena ou referencia os vértices aos quais está conectado.) - há muitas estações possíveis, mas cada uma se conecta a relativamente poucas outras.

&emsp;A **matriz de adjacência** teria muitas posições sem conexão, pois o grafo é esparso (poucas arestas em relação à quantidade máxima de conexões possíveis entre seus vértices): cada vértice se conecta diretamente a poucos outros vértices. Por isso, a **lista de adjacência** é mais adequada, pois armazena apenas as conexões existentes, reduzindo o uso de memória.

---

## Robô da CPTM:

&emsp;Navegação autônoma é a capacidade de perceber o ambiente, estimar a própria posição, planejar e executar movimentos sem depender de comandos humanos contínuos.

- **Percepção**: detectar pessoas, paredes e obstáculos.
- **Localização**: estimar a posição e a orientação do robô no mezanino.
- **Mapeamento**: representar corredores, áreas livres e obstáculos.
- **Planejamento**: definir a rota até um banheiro, elevador ou outro ponto de interesse.
- **Controle**: executar os movimentos necessários para percorrer a rota.

&emsp;*Localização* é estimar onde o robô está; *mapeamento* é representar o ambiente.

&emsp;*Percepção* coleta informações; o *planejamento* escolhe o caminho; e o controle executa os movimentos.

&emsp;SLAM - localização e mapeamento simultâneos.

&emsp;Não basta fornecer ao robô uma planta da estação pq não garante que o robô consiga determinar sua posição atual com precisão. ex: pessoas circulando, objetos bloqueando passagem e sensores podem conter ruído.

---

**Mapa métrico** - Representa geometria e medidas. Ajuda a localizar o robô e a realizar movimentos precisos.

**Mapa topológico** - Representa lugares e conexões entre eles. É útil para organizar rotas entre pontos de interesse.
pode ser representado como um grafo

&emsp;O grafo ajuda a decidir por onde ir, enquanto a representação geométrica auxilia na execução física da rota.

&emsp;O planejamento **global** responde: qual rota devo seguir até o destino?

&emsp;O planejamento **local** ou reativo responde: como devo me movimentar agora diante do que está acontecendo ao meu redor?

&emsp;*Exemplo*: o robô planejou chegar ao elevador passando por um corredor. Uma pessoa para no meio do caminho.

- O planejamento global continua indicando o destino e a rota pretendida.
- O comportamento local precisa reagir à pessoa, reduzindo a velocidade, desviando ou parando.
- Se a passagem ficar bloqueada, pode ser necessário replanejar a rota global.

&emsp;O ambiente da CPTM é **semiestruturado e dinâmico**: há corredores e acessos conhecidos, mas as pessoas circulam e podem modificar continuamente as condições do percurso.

&emsp;Rota pode não ser acessível para determinado passageiro.

---

## Sensores

&emsp;**Câmera** - Fornece imagens, cores e texturas. Ajuda a reconhecer objetos e características do ambiente, mas pode ter dificuldades com pouca luz ou superfícies sem textura

&emsp;**LiDAR** mede distâncias com laser, mas não fornece tanta informação visual, como cores.

&emsp;**Encoders** estimam o movimento das rodas - pode falhar quando as rodas derrapam.

&emsp;**IMUs** medem aceleração e rotação. útil para acompanhar mudanças de movimento

&emsp;Sensores diferentes podem se complementar, pois cada um fornece informações diferentes e possui limitações próprias. Servem para estimar melhor a posição do robô e entender o que está acontecendo ao redor. Com isso, consegue decidir como ajustar seu movimento.

---

**Por que não usar busca em largura?** Porque ela minimiza o número de arestas, não o tempo total em um grafo ponderado.

**Por que não usar busca em profundidade?** Porque ela não garante o caminho de menor custo.

**Por que não usar uma linha reta no mapa como rota?** Porque paredes, escadas, plataformas e conexões disponíveis restringem os caminhos fisicamente viáveis.

---

## Arquitetura

&emsp;Arquitetura divide o sistema em camadas com responsabilidades distintas. Isso reduz o acoplamento e facilita manutenção, testes e evolução.

&emsp;Orquestração coordena o atendimento, mas não executa pessoalmente todas as tarefas. Ela encaminha cada solicitação ao componente responsável.

&emsp;Pergunta informacional, pergunta de rotas, acompanhamento do robô, situacoes de emergência

#### Usar identificadores de pontos em vez de coordenadas?

&emsp;Imagine que a **orquestração** peça ao robô para chegar às coordenadas (2.5, 4.0).

&emsp;Essas coordenadas só fazem sentido em relação a um mapa específico. Se a plataforma ou o mapa mudar, a posição correspondente ao banheiro pode mudar também.

&emsp;A **orquestração** decide o que precisa acontecer e em qual ordem; a **abstração** define como pedir que o robô físico execute a ação, sem precisar conhecer os detalhes do hardware.

&emsp;*Exemplo*: Com **abstração**, a **orquestração** continua solicitando algo como “navegar até a bilheteria”. O adaptador da nova plataforma fica responsável por implementar essa solicitação usando a interface do novo robô.

&emsp;Requisito de modularidade e desacoplamento da plataforma robótica.

&emsp;A camada de **abstração** fica responsável por descobrir a posição correspondente naquele mapa do banheiro1 e acionar a navegação.

&emsp;A **orquestração** usa identificadores de pontos, enquanto a abstração resolve a posição correspondente no mapa.

&emsp;**Action no ROS 2** - permite enviar um objetivo, receber atualizações, obter um resultado e solicitar cancelamento.

&emsp;Comandos de parar e cancelar sejam aceitos em qualquer estado e tenham prioridade sobre os demais.