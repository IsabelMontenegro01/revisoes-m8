## Grafos

&emsp;Na Sprint 2, o grupo utilizou:
- A* para encontrar rotas entre estações.
- Dijkstra para encontrar caminhos dentro do mezanino.
- Um heap binário para organizar a fila de prioridade do A*.

#### Mas por que não utilizar BFS ou DFS?

&emsp;Imagine que o passageiro queira ir da estação Tamanduateí até a Luz.

&emsp;Cada conexão tem um tempo de viagem diferente. Além disso, existem tempos de espera e de baldeação.

&emsp; A **BFS** poderia encontrar um caminho com menos conexões, mas não necessariamente o mais rápido.
&emsp;A **DFS** poderia encontrar um caminho, mas não garante que seja o de menor custo.

&emsp;Por isso, para esse problema, os **algoritmos ponderados** são mais adequados.

&emsp;Isso não significa que BFS e DFS sejam inúteis no projeto. São algoritmos importantes para explorar grafos, verificar conectividade e resolver outros problemas.

---

## NAV2 

- Controlador
- Planejado 
- Navegador

--- 

## ROS2

&emsp;Imagine que um passageiro solicita ao robô que o conduza até um banheiro.

- Backend: Recebe a solicitação do passageiro e identifica o destino

- Envia um objetivo de navegação

- ROS 2: Faz a comunicação com os componentes robóticos

- Nav2: Planeja e executa a navegação até o destino

- Robô: Realiza o deslocamento físico ou simulado

&emsp;A navegação foi definida como uma action. Então, enquanto o robô executa a tarefa, o sistema pode receber eventos como chegada ao destino, interrupção por obstáculo, ausência de caminho alternativo ou perda de localização.