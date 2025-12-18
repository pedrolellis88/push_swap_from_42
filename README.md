*Este projeto foi criado como parte do currículo da 42 por **pdiniz-l**.*

## push_swap_from_42

### Descrição

O projeto **push_swap** é um trabalho da 42 School que consiste na implementação de um algoritmo de ordenação em C utilizando duas pilhas (**A** e **B**) e um conjunto restrito de **11 operações permitidas**.

O programa recebe uma lista de inteiros como entrada, realiza a validação desses dados e imprime na saída padrão a sequência de operações necessária para ordenar a pilha **A** em ordem crescente.  
O objetivo não é apenas a correção, mas também a eficiência, minimizando o número de operações utilizadas.

Este projeto foca em **raciocínio algorítmico**, **manipulação de estruturas de dados**, **validação de entrada** e **segurança de memória**.

---

### Instruções

#### Compilação

O projeto é compilado utilizando o `Makefile` fornecido.

A partir do diretório raiz do projeto:

```bash
make        # compila libft_applier e gera o binário push_swap
make clean  # remove arquivos objeto
make fclean # remove arquivos objeto e o binário
make re     # recompila tudo do zero
```
O Makefile primeiro compila a `libft` personalizada localizada em `libft_applier`/ e, em seguida, gera o executável `push_swap`.

### Requisitos

* Compilador C (cc, gcc ou clang)

* make

* Ambiente Unix-like (Linux / macOS)

* libft já incluída em libft_applier (não é necessária instalação separada)

### Uso

Entradas válidas podem ser fornecidas de duas formas:
```bash
./push_swap 2 1 3 6 5 8
./push_swap "2 1 3 6 5 8"
```

Se a entrada for válida, o programa imprime na saída padrão a sequência de operações, por exemplo:
```bash
./push_swap 2 1 3 
sa
```
Em caso de erro (argumento inválido, número duplicado, inteiro fora do intervalo permitido, etc.), o programa imprime:
```text
Error
```
em `stderr` e encerra com código de saída `1`.

#### Operações Permitidas

O programa pode utilizar apenas as seguintes operações:

* `sa`, `sb`, `ss` — swap : troca os dois primeiros elementos de uma pilha.

* `pa`, `pb` — push : move o elemento do topo de uma pilha para a outra.

* `ra`, `rb`, `rr` — rotate : desloca a pilha para cima (o primeiro elemento passa a ser o último).

* `rra`, `rrb`, `rrr` — reverse rotate : desloca a pilha para baixo (o último elemento passa a ser o primeiro).

#### Escolhas de Algoritmo e Estrutura de Dados
##### Representação das Pilhas

As pilhas são representadas utilizando uma lista encadeada da `libft`:
```c
typedef struct s_data
{
    t_list  *a;
    t_list  *b;
}   t_data;
```
Essa estrutura permite rotações e operações de push de forma eficiente, mantendo o uso de memória controlado.

##### Parsing e Validação

Antes da ordenação, o programa garante que:

* Todos os argumentos são inteiros válidos;

* Os valores estão dentro do intervalo de int (32 bits);

* Não existem números duplicados.

##### Principais funções de parsing e validação:

`parse_args` — separa os argumentos e constrói a pilha A

`is_str_valid` — valida se a string representa um inteiro válido

`safe_atoi` — converte char * para int com checagem de overflow

`is_doubled` — verifica a existência de valores duplicados

`error_and_exit` — libera a memória, imprime Error e encerra o programa

##### Indexação (Compressão de Coordenadas)

Para simplificar a ordenação:

* Todos os valores são copiados para um array.

* O array é ordenado.

* O valor de cada nó é substituído pelo seu índice no array ordenado.

Isso reduz o problema para ordenar valores de 0 a n - 1, o que é mais adequado para algoritmos eficientes como o radix sort.

##### Estratégias de Ordenação

* Casos Pequenos (2–5 elementos)

Funções específicas tratam entradas pequenas utilizando combinações mínimas de operações:

`sort_two`

`sort_three`

`sort_four`

`sort_five`

Esses casos são tratados de forma direta para obter desempenho ótimo.

* Casos Médios (≤ 100 elementos)

Ordenação baseada em chunks:

Os índices são divididos em intervalos (chunks);

Elementos do chunk atual são enviados para a pilha B usando rotações;

Os elementos são reinseridos de B para A, sempre trazendo o maior valor de B para o topo.

Essa abordagem reduz significativamente o número de operações.

* Casos Grandes (> 100 elementos)

Radix sort binário sobre os índices:

Para cada bit:

Percorre a pilha A;

Envia para a pilha B os elementos cujo bit atual é 0;

Em seguida, retorna todos os elementos de B para A.

O processo é repetido até que todos os bits relevantes sejam processados, com complexidade aproximada de O(n log n).

#### Estrutura do Repositório
```text
push_swap_from_42/
├── libft_applier/      # libft personalizada (libft.a + headers)
├── srcs/               # código-fonte do push_swap
│   ├── push_swap.h
│   ├── main.c
│   ├── parse_args.c
│   ├── create_head_with_ints.c
│   ├── is_str_valid.c
│   ├── safe_atoi.c
│   ├── is_doubled.c
│   ├── is_list_sorted.c
│   ├── error_and_exit.c
│   ├── swap_utils.c
│   ├── push_utils.c
│   ├── rotate_utils.c
│   ├── reverse_rotate_utils.c
│   ├── two_three_five_sort_utils.c
│   ├── sort_five_especific_utils.c
│   ├── indexing1.c
│   ├── indexing2.c
│   └── chunk_sort.c
└── Makefile
```
### Recursos

42 School — enunciado do push_swap

#### Algoritmos:

* Radix Sort

* Estratégias de ordenação por chunks

* Biblioteca Padrão C

#### Documentação POSIX:

* write(2)

* stdlib(3)

# English Version:

*This project has been created as part of the 42 curriculum by **pdiniz-l**.*

## push_swap_from_42

### Description

The **push_swap** project is a 42 School assignment that consists of implementing a sorting algorithm in C using two stacks (**A** and **B**) and a restricted set of **11 allowed operations**.

The program receives a list of integers as input, validates them, and prints to standard output the sequence of operations required to sort stack **A** in ascending order.  
The objective is not only correctness, but also efficiency, minimizing the number of operations used.

This project focuses on **algorithmic thinking**, **data structure manipulation**, **input validation**, and **memory safety**.

---

### Instructions

#### Compilation

The project is compiled using the provided `Makefile`.

From the project root directory:

```bash
make        # builds libft_applier and the push_swap binary
make clean  # removes object files
make fclean # removes object files and the binary
make re     # full rebuild
```
The Makefile first compiles the custom libft located in libft_applier/ and then builds the push_swap executable.

## Requirements

* C compiler (cc, gcc, or clang)

* make

* Unix-like environment (Linux / macOS)

* libft is already included in libft_applier (no separate installation required)

## Usage

Valid inputs can be provided in two ways:
```bash
./push_swap 2 1 3 6 5 8
./push_swap "2 1 3 6 5 8"
```

If the input is valid, the program prints to standard output the sequence of operations, for example:
```bash
./push_swap 2 1 3 
sa
```

In case of error (invalid argument, duplicate number, integer out of range, etc.), the program prints:
```text
Error
```
to `stderr` and exits with status code `1`.

### Allowed Operations

The program may use only the following operations:

* `sa`, `sb`, `ss` — swap : Swaps the first two elements of a stack.

* `pa`, `pb` — push : Moves the top element from one stack to the other.

* `ra`, `rb`, `rr` — rotate : Shifts the stack up: the first element becomes the last.

* `rra`, `rrb`, `rrr` — reverse rotate : Shifts the stack down: the last element becomes the first.

### Algorithm and Data Structure Choices
#### Stack Representation

The stacks are represented using a linked list from `libft`:
```c
typedef struct s_data
{
    t_list  *a;
    t_list  *b;
}   t_data;
```
This structure allows efficient rotations and pushes while keeping memory usage controlled.

### Parsing and Validation

Before sorting, the program ensures:

* All arguments are valid integers;

* Values fit within the 32-bit int range;

* There are no duplicate numbers.

#### Main parsing and validation helpers:

* `parse_args` — splits arguments and builds stack A

* `is_str_valid` — validates integer strings

* `safe_atoi` — converts char * to int with overflow checking

* `is_doubled` — checks for duplicate values

* `error_and_exit` — frees memory, prints Error, and exits

#### Indexing (Coordinate Compression)

To simplify sorting:

* All values are copied into an array.

* The array is sorted.

* Each node value is replaced by its index in the sorted array.

This reduces the problem to sorting values from 0 to n - 1, which is more suitable for efficient algorithms such as radix sort.

##### Sorting Strategies
* Small Cases (2–5 elements)

Specific functions handle small inputs using minimal combinations of operations:

`sort_two`

`sort_three`

`sort_four`

`sort_five`

These cases are hardcoded for optimal performance.

* Medium Cases (≤ 100 elements)

Chunk-based sorting:

Indices are divided into chunks (intervals).

Elements belonging to the current chunk are pushed to stack B using rotations.

Elements are pushed back from B to A, always bringing the largest value to the top.

This approach significantly reduces the number of operations.

* Large Cases (> 100 elements)

Binary radix sort on indices:

For each bit:

Traverse stack A

Push elements with the current bit set to 0 into stack **B`

Push everything back from B to A

This process is repeated until all relevant bits are processed, with time complexity approximately O(n log n).

#### Repository Structure
```text
push_swap_from_42/
├── libft_applier/      # Custom libft (libft.a + headers)
├── srcs/               # push_swap source code
│   ├── push_swap.h
│   ├── main.c
│   ├── parse_args.c
│   ├── create_head_with_ints.c
│   ├── is_str_valid.c
│   ├── safe_atoi.c
│   ├── is_doubled.c
│   ├── is_list_sorted.c
│   ├── error_and_exit.c
│   ├── swap_utils.c
│   ├── push_utils.c
│   ├── rotate_utils.c
│   ├── reverse_rotate_utils.c
│   ├── two_three_five_sort_utils.c
│   ├── sort_five_especific_utils.c
│   ├── indexing1.c
│   ├── indexing2.c
│   └── chunk_sort.c
└── Makefile
```
### Resources

42 School — push_swap subject

#### Algorithms:

* Radix Sort

* Chunk-based sorting strategies

* C Standard Library

#### POSIX documentation:

* write(2)

* stdlib(3)
