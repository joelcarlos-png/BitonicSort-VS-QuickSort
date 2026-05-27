# Análise de resultados observados em [NotebookColab](../NotebookColab.ipynb)

Ao compilar o código em "[NotebookColab](../NotebookColab.ipynb)", observamos que a velocidade que cada Sort
ordena uma lista de 1.048.576 elementos é variável, mas em geral o Bitonic Sort realizado com o poder da GPU é
bem mais rápido que o Quick Sort realizado na CPU, isso acontece devido ao seguinte:

### Explicação da superioridade do Bitonic Sort na GPU

Como a CPU, que é a força bruta do Quick Sort, tem poucos núcleos (geralmente de 2 a 16) que são muito otimizados
para lógica e baixa latência, isso ainda assim é um trabalho sequencial, isso que muda tudo. No Bitonic Sort, a
GPU usa milhares de threads simultâneas, e também a lógica do Bitonic Sort ajuda, pois ele acessa os dados de uma
forma estruturada, evitando que a GPU entregue dados na ordem errada. Essa lógica ajuda a GPU a fazer um coalescing,
que é combinar vários acessos na memória em uma única transação. Usando uma analogia, é como se o Quick Sort fosse
um pedreiro que coloca tijolos muito rápido, porém em sequência, e o Bitonic Sort na GPU é como se fossem vários pedreiros
(nesse código são 512) colocando um tijolo cada um.

### Gráfico para melhor entendimento da superioridade do Bitonic Sort: [Gráfico](GraficoBitonicSort-VS-QuickSort.png)
