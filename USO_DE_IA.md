# Uso de IA declarado

Foi usado IA para desenvolvimento assistido do código Bitonic Sort em CUDA e também para o código do Quick Sort na CPU.
Usei o modelo do Gemini 3.1 PRO com o seguinte prompt:

```text
"faça um algoritmo de Bitonic sort em GPU(CUDA) e um Quick sort em CPU, ambos em C, usando essas instruções abaixo
e faça os dois printarem o tempo de execução no final e mostrar a razão do mais rápido para o mais devagar

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

---

### Uso de IA para fazer Gráfico

Foi usado IA para fazer o [gráfico](Resultados/GraficoBitonicSort-VS-QuickSort.png) personalizado, usando novamente o
Gemini 3.1 PRO com o seguinte prompt:

```text
Resumo detalhado dos resultados do código:
Descrição do Experimento: CPU vs GPU Sorting

1. O Cenário:

Volume de Dados: O código processou um array de 1.048.576 elementos (2^20), preenchidos com números inteiros aleatórios.

Algoritmo CPU: Quick Sort (Implementação serial recursiva).

Algoritmo GPU: Bitonic Sort (Implementação paralela utilizando CUDA).

2. Resultados Obtidos (Performance):

Tempo de Execução (CPU): 0.202804 segundos.

Tempo de Execução (GPU): 0.007907 segundos (incluindo a transferência de memória e processamento paralelo).

Razão de Desempenho (Speedup): A GPU foi aproximadamente 25.65x mais rápida que a CPU.

3. Verificação de Integridade:

Ambos os algoritmos produziram resultados idênticos, confirmando o sucesso da ordenação e a corretude da lógica paralela.

4. Sugestão para o Gráfico:

Tipo de Gráfico: Barras comparativas.

Eixo Y: Tempo em segundos (escala logarítmica pode ser útil para destacar a diferença drástica).

Anotação: Incluir o selo '25.65x Faster' sobre a barra da GPU.
```

Ela me retornou o seguinte script em Python:

```text
import matplotlib.pyplot as plt
from matplotlib.ticker import FuncFormatter

# 1. Dados do experimento
labels = ['CPU\n(Quick Sort)', 'GPU\n(Bitonic Sort)']
tempos = [0.202804, 0.007907]
cores = ['#3498db', '#2ecc71']  # Azul para CPU, Verde para GPU

# 2. Configuração inicial do gráfico
plt.figure(figsize=(8, 6))
barras = plt.bar(labels, tempos, color=cores, width=0.5)

# 3. Configurando os eixos e títulos
plt.yscale('log') # Escala logarítmica solicitada
plt.ylabel('Tempo de Execução (segundos) - Escala Log', fontsize=12)
plt.title('Performance de Ordenação: 1.048.576 Elementos\nCPU vs GPU', fontsize=14, fontweight='bold', pad=20)

# Formata os valores do eixo Y em decimal, sem notação científica.
plt.gca().yaxis.set_major_formatter(FuncFormatter(lambda valor, _: f'{valor:.3f}'.replace('.', ',')))

# 4. Adicionando os tempos exatos sobre as barras
for barra in barras:
    yval = barra.get_height()
    # Adicionando um pequeno offset multiplicativo por causa da escala logarítmica
    plt.text(barra.get_x() + barra.get_width()/2, yval * 1.1, f'{yval:.6f}s',
             ha='center', va='bottom', fontsize=11, fontweight='bold')

# 5. Adicionando a anotação "25.65x Faster"
# Posicionado acima da barra da GPU (índice 1)
plt.annotate('25.65x Faster!',
             xy=(1, tempos[1] * 1.5),         # Ponto de destino da seta
             xytext=(1, tempos[1] * 10),      # Posição do texto (bem acima devido ao log)
             ha='center', fontsize=13, fontweight='bold', color='#c0392b',
             bbox=dict(boxstyle="round,pad=0.3", edgecolor='#c0392b', facecolor='#fadbd8'),
             arrowprops=dict(facecolor='#c0392b', shrink=0.05, width=2, headwidth=8))

# 6. Ajustes visuais finais
plt.grid(axis='y', linestyle='--', alpha=0.6)
plt.gca().set_axisbelow(True) # Coloca a grade atrás das barras
plt.tight_layout()

# 7. Salvar e exibir
plt.savefig('grafico_cpu_vs_gpu.png', dpi=300)
print("Gráfico salvo como 'grafico_cpu_vs_gpu.png'")
plt.show()
```

Baixando a biblioteca matplotlib com `pip install matplotlib` e executando o código, é retornado o gráfico.

### Uso de IA nos Slides

Eu usei duas IAs: o Notebook lm para pegar inspiração para os slides e o GPT-5.5 para criação de PNGs para o slide.

Obs.: o slide foi feito por mim, porém com auxílio da IA para pequenas coisas.

## Reflexão Joel Carlos | O que fez: Código principal, Slide e GitHub

Eu, particularmente, digo que a IA generativa foi imprescindível tanto no desenvolvimento do código como na parte visual,
como gráficos e slides. Sem a IA, teria sido um trabalho muito árduo e com menor qualidade. A IA ainda comete muitos erros,
mas ela entrega um resultado muito satisfatório quando você está acompanhando e fazendo junto com o que ela propõe e implementa.
Você sempre tem que revisar e entender o que ela está tentando passar, mas é uma ferramenta muito importante, principalmente nesse tipo
de trabalho.
