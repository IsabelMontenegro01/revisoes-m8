**1. Representação de grafos**

Calcular rotas numa rede de trens começa por representar essa rede como um grafo. Responda aos itens a seguir.

DICA: Um grafo é esparso quando cada vértice tem aresta direta para poucos outros, de modo que existem muito menos arestas do que existiriam se todo par de vértices tivesse uma aresta. É denso quando quase todo par de vértices tem uma aresta direta. Essa diferença pesa mais numa das duas formas de representar um grafo.

* **A)** Explique a diferença entre um grafo dirigido e um não dirigido e o que muda na matriz de adjacência de cada um.


* **B)** Analise a afirmação a seguir, dizendo se ela é verdadeira ou falsa como um todo e justificando conceitualmente a sua resposta: "Para um grafo com milhares de vértices e poucas arestas por vértice, a matriz de adjacência é a melhor escolha para calcular rotas, porque verifica se uma aresta existe em tempo constante."


* **C)** No grafo do seu grupo, a baldeação liga os vértices (estação, linha) de um mesmo complexo com o mesmo tempo nos dois sentidos, e o arquivo do grafo guarda cada ligação como duas arestas dirigidas. Avalie se a baldeação deveria ter custo diferente em cada sentido, o que exige aresta dirigida, considerando o que acontece com o passageiro numa troca de linha.



---

**2. Navegação autônoma**

Um robô que orienta passageiros precisa se mover com segurança num ambiente cheio de pessoas e que muda o tempo todo. Responda aos itens a seguir.

* **A)** Além de indicar onde o robô pode passar, o que mais o mapa precisa representar sobre cada trecho para que o planejamento possa comparar caminhos e escolher o melhor, e não apenas um caminho possível? Explique.


* **B)** Analise a afirmação a seguir, dizendo se ela é verdadeira ou falsa como um todo e justificando conceitualmente a sua resposta: "Um veículo de armazém que segue uma linha pintada no chão é tão autônomo quanto um robô que navega sem infraestrutura, porque os dois chegam sozinhos ao destino."


* **C)** Na proposta do seu grupo, o robô parte de um ponto fixo perto da SSO, conduz o passageiro até um ponto de interesse do mezanino e reconhece a chegada pela localização no mapa. Avalie se o caminho mais curto no mapa é sempre o melhor caminho para o robô na proposta do seu grupo e que custos, além da distância, o planejamento deveria considerar.