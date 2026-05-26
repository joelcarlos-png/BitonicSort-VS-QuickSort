# Bitonic Sort (GPU) - VS - Quick Sort (CPU)

Este tem o intuito de comparação entre o paralelismo de uma GPU vs. a velocidade sequencial de uma CPU em ordenação de listas numéricas (INT). Nós escolhemos o Bitonic Sort na GPU e o Quick Sort na CPU.

---

## Conteúdo

O projeto está estruturado em vários módulos principais:

### 1. [NotebookColab](https://www.google.com/search?q=NotebookColab.ipynb) <- Código do Bitonic Sort e Quick Sort.

* **Ordenação de lista:** Quantidade de uma lista de 1.048.576 elementos escolhida para estressar CPU e GPU.
* **Medição de velocidade:** Mede a velocidade que cada algoritmo levou para realizar a tarefa.
* **Cálculo da razão:** Mede quantas vezes o mais rápido foi que o mais lento.

### 2. Pasta [Resultados](https://www.google.com/search?q=Resultados) <- Análise de resultados do código acima.

* **[RESULTADOS.md:](https://www.google.com/search?q=Resultados/RESULTADOS.md)** Markdown explicando o porquê da superioridade do Bitonic Sort na GPU.
* **[Gráfico:](https://www.google.com/search?q=Resultados/GraficoBitonicSort-VS-QuickSort.png)** Gráfico mostrando o tempo que cada sort demorou para concluir a tarefa.

---

## Tecnologias e Conceitos Aplicados

* **Linguagem:** C
* **Arquitetura CUDA (Compute Unified Device Architecture):** Utilizando a plataforma da NVIDIA, permitindo que o hardware gráfico calcule cálculos matemáticos.
* **Programação Heterogênea:** Utilização de gerenciamento de memória através de `cudaMalloc`, `cudaMemcpy` e `cudaFree`.
* **Tipo de execução SIMT (Single Instruction, Multiple Threads):** O kernel `bitonic_sort_step` usa milhares de threads simultaneamente.
* **Sincronização de threads:** O uso de `cudaDeviceSynchronize` garante que o resultado das threads esteja em ordem correta.
* **Operações Bitwise:** Uso de operadores de baixo nível (`^`, `&`, `<<`) para cálculo de índice dentro de cada thread.

---

## Estrutura de Pastas

```text
.
├── Resultados/             # Análise de resultados
├── NotebookColab.ipynb     # Implementação dos algoritmos propostos
├── USO_DE_IA.md            # Diz respeito a todo o uso de IA generativa no projeto
├── LICENSE                 # Licença MIT do projeto
└── README.md               # Documentação do projeto

```

## Como Executar o Projeto

Siga os passos abaixo:

1. **Clone o repositório:**
```bash
git clone https://github.com/joelcarlos-png/Estrutura_de_Dados_em_C.git
```
2.  **Acesse o arquivo [NotebookColab.ipynb](NotebookColab.ipynb):**
```bash
crie um arquivo.c com o código
```
3. **Compile os arquivos fonte:**
```bash
nvcc sorts_cpu_gpu.cu -o sorts_cpu_gpu
```
4.  **Execute o binário gerado:**
```bash
./sorts_cpu_gpu
```
---

## Autor

Desenvolvido por **Joel Carlos** e **Bryan Cruz**

* LinkedIn Joel: [www.linkedin.com/in/joelcarlosassuncaopadilha]()
* GitHub Joel: https://github.com/joelcarlos-png
* =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
* LinkedIn Bryan: [www.linkedin.com/in/bryanmarquescruz/]()
* GitHub Bryan: [https://github.com/bryantlanta-alt]()

---

*Este projeto foi criado para um trabalho acadêmico proposto pelo professor Newarney Torrezao da Costa.*

***Colégio:** Instituto Federal Goiano - Campus Iporá.*

***Curso:** Bacharelado em Ciência da Computação.*
