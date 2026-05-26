# Uso de IA declarado

Foi usado IA para desenvolvimento assistido do codigo Bitonic Sort em CUDA e tbm para o codigo do Quick sort na CPU
usei o modelo do Gemini 3.1 PRO com o seguinte prompt:

```text
"faça um algoritmo de Bitonic sort em GPU(CUDA) e um Quick sort em CPU, ambos em C, usando essas intrucoes abaixo
e faça os dois printar o tempo de execucao no final e mostrar a razao do mais rapido para o mais devagar

Comparação de Algoritmos: CPU vs GPU
Para avançarmos no processamento paralelo, vamos implementar dois algoritmos de ordenação distintos.

1. Quicksort (CPU)
O Quicksort é um algoritmo de divisão e conquista.

Como funciona: Ele escolhe um elemento como 'pivô' e particiona o array, colocando todos os elementos menores que o
pivô à esquerda e os maiores à direita. Depois, ele se chama recursivamente para as duas metades.
Por que no CPU? A recursão e a escolha dinâmica do pivô são fáceis de gerenciar em arquiteturas seriais. Em GPUs,
a recursão profunda pode causar problemas de memória (stack overflow) e baixa eficiência.
2. Bitonic Sort (GPU)
O Bitonic Sort é um Sorting Network (Rede de Ordenação).

Como funciona: Ele foca em criar 'sequências bitônicas' (uma sequência que cresce e depois diminui). Através de várias
etapas de comparação e troca (compare-and-swap), ele transforma essas sequências em uma única lista ordenada.
Por que no GPU? Diferente do Quicksort, o Bitonic Sort faz as mesmas comparações independentemente dos valores dos dados.
Isso significa que as threads da GPU podem trabalhar de forma perfeitamente sincronizada, sem que uma thread precise esperar
 a decisão de outra.
O que você precisará de CUDA para o Bitonic Sort
Para implementar o Bitonic Sort, você precisará ir além do printf. Aqui estão os novos conceitos de sintaxe e arquitetura:

1. Gerenciamento de Memória Real (cudaMalloc e cudaMemcpy)
Até agora, passamos apenas números simples para o Kernel. Para ordenar, precisamos passar um array inteiro. Como a GPU tem
sua própria memória (VRAM), você deve:

Alocar espaço na GPU: cudaMalloc((void**)&d_array, tamanho);
Copiar do CPU para GPU: cudaMemcpy(d_array, h_array, tamanho, cudaMemcpyHostToDevice);
Copiar de volta após ordenar: cudaMemcpy(h_array, d_array, tamanho, cudaMemcpyDeviceToHost);
2. Sincronização de Threads (__syncthreads())
O Bitonic Sort funciona em 'etapas'. Você não pode começar a etapa 2 antes que todas as threads tenham terminado a etapa 1.
Dentro de um bloco, usamos a barreira:

__syncthreads(); -> Faz com que todas as threads do bloco esperem umas pelas outras antes de prosseguir.
3. Operações Bitwise (E/OU/XOR)
A lógica de saber qual thread compara com qual no Bitonic Sort envolve manipulação de bits (usando os operadores &, |, ^, <<).
Isso é fundamental para calcular os índices de comparação baseados no threadIdx.x.

4. Kernels Iterativos
Como o Bitonic Sort tem várias passagens, você frequentemente chamará o Kernel várias vezes de dentro de um loop no main() (CPU),
ou usará loops complexos dentro da GPU."
````


