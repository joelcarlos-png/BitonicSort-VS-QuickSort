# Bitonic Sort (GPU) - VS - Quick Sort (CPU)
Este tem o intuido de comparaçao entre o paralelismo de uma GPU Vs a velocidade sequencial de uma CPU em ordencao de listas numericas(INT),
nos ecolhemos o Bitnic Sort na GPU e o Quick Sort na CPU.

---

## Conteúdo

O projeto está estruturado em vários módulos principais:

### 1. [NotebookColab](NotebookColab.ipynb) <- Codigo do Bitonic Sort e Quick Sort.
* **Ordenacao de lista:** Quantidade de uma lista 1.048.576 elementos escolhida para extressar CPU e GPU.
* **Mediçao de velocidade:** Mede a velocidade que cada algoritmo levou para realizar a tarefa.
* **Calculo da razao:** Mede quantas vezes o mais rapido foi que o mais lento.

### 2. Pasta [Resultados](Resultados) <- Analise de resultados do codigo acima.
* **[RESULTADOS.md:](Resultados/RESULTADOS.md)** markdow explicando o porque da superioridade do Bitonic Sort na GPU.
* **[Grafico:](Resultados/GraficoBitonicSort-VS-QuickSort.png)** Grafico mostrando o tempo que cada Sort demorou para concluir a tarefa.
---

## Tecnologias e Conceitos Aplicados

* **Linguagem:** C
* **Arquitetura CUDA (Compute Unified Device Architecture):** Utilizando a plataforma da NVIDEA permitindo que o hardware grafico
   calcule calculos matematicos.
* **Progamaçao Heterogena:** Utilizacao de gerenciamento de memoria atravez de `cudaMalloc`, `cudaMemcpy` e `cudaFree`.
* **Tipo de execucao SIMT (Single instruction, Multiple threads):** O kernel `bitonic_sort_step` usa milhares de threads simultaneamente.
* **Sincronizaçao de threads:** Uso de `cudaDeviceSynchronize` garante que o resultado dos threads estejam em ordem correta.
* **Operaçoes Bitwise:** Uso de operadores de baixo nivel(`^`, `&`, `<<`) para calculo de indice dentro de cada thread.

---

## Estrutura de Pastas

```text
.
├── Resultados/              # Analise de resultados
├── NotebookColab.ipynb      # Implementação dos algoritmos propostos
├── USO_DE_IA.md             # Diz respeito a todo o uso de IA generativa no projeto
├── LICENSE                  # licença MIT do projeto
└── README.md                # Documentação do projeto
````

## 🚀 Como Executar o Projeto

Para testar qualquer uma das estruturas, siga os passos abaixo:

1.  **Clone o repositório:**

    ```bash
    git clone https://github.com/joelcarlos-png/Estrutura_de_Dados_em_C.git
    ```

2.  **Acesse o arquivo [NotebookColab.ipynb](NotebookColab.ipynb):**

    ```bash
    crie um arquivo.c com o codigo
    ```

3.  **Compile os arquivos fonte:**

    ```bash
    nvcc sorts_cpu_gpu.cu -o sorts_cpu_gpu
    ```

4.  **Execute o binário gerado:**

    ```bash
    ./sorts_cpu_gpu
    ```

-----

## Autor

Desenvolvido por **Joel Carlos** e **Bryan Cruz**
  * LinkedIn Joel: [www.linkedin.com/in/joelcarlosassuncaopadilha](https://www.linkedin.com/in/joelcarlosassuncaopadilha/)
  * GitHub Joel: https://github.com/joelcarlos-png
  * =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
  * LinkedIn Bryan: [www.linkedin.com/in/joelcarlosassuncaopadilha](https://www.linkedin.com/in/bryanmarquescruz/)
  * GitHub Bryan: [https://github.com/joelcarlosap321-png](https://github.com/bryantlanta-alt)


-----

*Este projeto foi criado para um Trabalho Academico proposto pelo professor Newarney Torrezao da Costa.*

***Colegio:** Instituto Federal Goiano - Campus Iporá.*

***Curso:** Bacharelado em Ciencias da Computaçao.*
