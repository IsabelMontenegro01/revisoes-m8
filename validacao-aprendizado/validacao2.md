# Validação de Aprendizado — Sprint 2
**M8 EC · Navegação autônoma**
* **Grupo:** G4
* **Aluno(a):** Isabel Montenegro Ruela

---

## 1. Grafos
*(cada item vale 0,1 — questão vale 0,3)*

Encontrar a rota de um passageiro na rede da CPTM é percorrer um grafo. Responda aos itens a seguir.

**A)** Explique o que é uma **árvore geradora mínima** e como o algoritmo de **Prim** a constrói.

**B)** Analise a afirmação a seguir, dizendo se ela é verdadeira ou falsa como um todo e justificando conceitualmente a sua resposta: *"Na **árvore geradora mínima** de uma rede de trens, o caminho entre duas estações é sempre o **caminho mais curto** entre elas, porque a árvore só usa as arestas de menor peso."*

**C)** No projeto do seu grupo, não há árvore geradora mínima: o grafo é consultado a cada pedido para achar o caminho de menor tempo (ou de menos baldeações) entre a origem e o destino, e cada consulta é resolvida de forma independente. **Avalie** se uma **árvore geradora mínima** da rede serviria para responder às consultas de rota do seu projeto e que problema ela resolveria no lugar do caminho mínimo.

---

## 2. Linguagem
*(cada item vale 0,1 — questão vale 0,3)*

Para entender as dúvidas dos passageiros, o assistente precisa transformar texto em números. Responda aos itens a seguir.

**A)** Explique a **hipótese distribucional** e como o **word2vec (skip-gram)** aprende um vetor para cada palavra a partir dela.

**B)** Analise a afirmação a seguir, dizendo se ela é verdadeira ou falsa como um todo e justificando conceitualmente a sua resposta: *"No **word2vec**, o vetor de rei − homem + mulher é exatamente o vetor de rainha, o que mostra que os embeddings guardam as relações entre palavras de forma exata."*

**C)** No projeto do seu grupo, não há word2vec nem embedding em uso: o FAQ reconhece sinônimos por listas de palavras-chave escritas à mão e por semelhança de prefixo, enquanto a coleção vetorial do Qdrant já está criada com vetor denso e esparso, mas vazia e sem modelo de embeddings. **Avalie** como **embeddings** treinados com word2vec mudariam o tratamento de **sinônimos** no FAQ do seu grupo, e o que se ganharia e o que se perderia.