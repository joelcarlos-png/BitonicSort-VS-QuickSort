# Análise de resultados observados em [NotebookColab](../NotebookColab.ipynb)

Ao compilar o codigo em "[NotebookColab](../NotebookColab.ipynb)" observamos que a velocidade que cada Sort
ordena uma lista de 1.048.576 elementos é variavel, mas em geral o Bitonic Sort realizado com o poder da GPU é
bem mais rapido que o Quick sort realizado na CPU, isso acontece devido ao seguinte:

### Explicaçao da superioridade do Bitonic Sort na GPU

Como a CPU que e a força bruta do Quick Sort tem poucos nucleos(geralmente de 2 a 16) que sao muito otimizados
para logica e baixa latencia, isso ainda assim e um trabalho sequencial, isso que muda tudo, no Bitonic Sort, a
GPU usa milhares de Threads simultaneos, e tbm a logica do Bitonic Sort ajuda pois ele acessa os dados de uma
forma estruturada, evitando que a GPU entregue dados na ordem errada, essa logica ajuda a GPU a fazer um coalescing
que é combinar vários acessos na memória em uma única transação usando uma analogia, e como se o Quick sort fosse 
um pedreiro que coloca tijolos muito rapido porem em sequencia, e o Bitonic sort na GPU e como se fosse varios pedreiros
(nesse codigo sao 512) colocando um tijolo cada um, bem mais rapido né

### Grafico para melhor entendimento da superioridade do Bitonic sort: [Grafico](GraficoBitonicSort-VS-QuickSort.png)
