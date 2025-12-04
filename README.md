# push_swap_from_42

Projeto da 42 que implementa um algoritmo de ordenação em C utilizando duas pilhas (**A** e **B**) e um conjunto limitado de 11 operações.  
O programa recebe uma lista de inteiros, valida a entrada e imprime na saída padrão a sequência de operações necessária para ordenar a pilha **A** em ordem crescente.

---

## 🎯 Objetivo

- Ordenar uma pilha de números inteiros usando **apenas** as operações permitidas:
  - `sa`, `sb`, `ss` – *swap*  > Troca o primeiro elemento da lista com o segundo elemento.
  - `pa`, `pb` – *push*  > Move o primeiro elemento da pilha de origem para o topo da pilha de destino.
  - `ra`, `rb`, `rr` – *rotate*  > Rotaciona a lista em 1 posição para cima: o primeiro elemento vai para o fim.
  - `rra`, `rrb`, `rrr` – *reverse rotate*  > Rotaciona a lista em 1 posição para baixo: o último elemento vai para o início.

- Garantir:
  - ausência de números duplicados;
  - todos os inteiros dentro do range de int (32 bits);
  - tratamento correto de erros e de *memory leaks*.

---

## Representação das pilhas

As pilhas são representadas por uma lista encadeada da `libft`:
```c
typedef struct s_data
{
    t_list  *a;
    t_list  *b;
}   t_data;

Principais grupos de funções:

-Parsing & validação

*parse_args – faz o split dos argumentos e monta a pilha A.

*is_str_valid – valida se a string representa um inteiro válido.

*safe_atoi – converte char * para int com checagem de overflow.

*is_doubled – verifica se há números duplicados.

*error_and_exit – limpa memória, imprime Error em stderr e encerra o programa.

Operações do projeto:

*sa, sb, ss – troca os dois primeiros elementos da pilha.

*pa, pb – move o topo de uma pilha para a outra.

*ra, rb, rr – desloca a pilha para cima (primeiro vai para o fim).

*rra, rrb, rrr – desloca a pilha para baixo (último vai para o início).

Ordenação:

-Casos pequenos

*Funções: sort_two, sort_three, sort_four, sort_five.

*Helpers: find_smallest, push_smallest_to_top, etc.

Indexação:

*Cria um array com todos os valores.

*Ordena o array.

*Substitui o valor de cada nó pelo seu índice nesse array ordenado.

*Isso reduz o problema a trabalhar com valores de 0 a n - 1, o que facilita os algoritmos de ordenação (como radix).

-Casos médios (≤ 100 elementos)

*chunk_sort

*Divide os índices em chunks (intervalos).

*Empurra para a pilha B os elementos do chunk atual usando rotações.

*Reinsere de B para A trazendo sempre o maior elemento de B para o topo.

-Casos grandes (> 100 elementos)

*radix

*Implementa radix sort em base 2 sobre os índices.

Para cada bit:

*percorre a pilha A;

*envia para B os elementos cujo bit atual é 0;

*depois traz todos de volta para A.

*Repete até processar todos os bits necessários, com complexidade aproximadamente O(n log n).

Estrutura do repositório
text
Copy code
push_swap_from_42/
├── libft_applier/      # Minha implementação de libft (libft.a + headers)
├── srcs/               # Código-fonte do push_swap
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
O Makefile compila a libft dentro de libft_applier/ e depois gera o binário push_swap.

Requisitos
Compilador C (cc, gcc ou clang);

make;

Ambiente Unix-like (Linux / macOS);

libft já incluída em libft_applier/
(não é necessário instalar separadamente — o Makefile cuida disso).

-Compilação

No diretório raiz do projeto:

make        # compila libft_applier e gera o binário push_swap
make clean  # remove arquivos objeto
make fclean # remove arquivos objeto e o binário
make re     # recompila tudo do zero
▶️ Uso
Entradas válidas
Você pode passar os números de duas formas:

./push_swap 2 1 3 6 5 8
./push_swap "2 1 3 6 5 8"
Se tudo estiver certo, o programa imprime na saída padrão a sequência de operações, por exemplo:

./push_swap 2 1 3
sa
Em caso de erro (número fora do range, argumento inválido, duplicado etc.), o programa imprime:

Error
em stderr e encerra com código de saída 1.

🔍 Exemplos
Contar o número de operações

./push_swap 4 67 3 87 23 9 1 0 | wc -l
Gerar entradas aleatórias e testar com um checker externo

# exemplo com números de 0 a 99 e lista com 50 elementos
ARG=$(seq 0 99 | shuf -n 50 | tr '\n' ' ')
./push_swap $ARG | ./checker_OS $ARG      # verifica se os comandos realmente ordenam a lista
./push_swap $ARG | wc -l               # mostra quantos comandos foram usados
(substitua o checker pelo script/programa de verificação que você estiver usando).

🧪 Estratégias de ordenação (resumo)
2 a 5 elementos

Casos específicos com combinações mínimas de sa, ra, rra, pa, pb.

Até 100 elementos

Compressão de valores em índices (0..n-1);

Divisão em chunks;

Empurra elementos de cada chunk para B;

Reinsere sempre trazendo o maior de B para o topo de A.

Mais de 100 elementos

Radix sort binário:

para cada bit, envia para B os elementos com bit 0;

no fim de cada passada, traz todos de volta para A;

repete até que todos os bits relevantes tenham sido processados.

ENGLISH VERSION:

# push_swap_from_42

42 project that implements a sorting algorithm in C using two stacks (**A** and **B**) and a limited set of 11 operations.  
The program receives a list of integers, validates the input and prints to standard output the sequence of operations required to sort stack **A** in ascending order.

---

## 🎯 Goal

- Sort a stack of integers using **only** the allowed operations:
  - `sa`, `sb`, `ss` – *swap*  
    > Swaps the first element of the stack with the second one.
  - `pa`, `pb` – *push*  
    > Moves the first element of the source stack to the top of the destination stack.
  - `ra`, `rb`, `rr` – *rotate*  
    > Rotates the stack up by 1 position: the first element becomes the last.
  - `rra`, `rrb`, `rrr` – *reverse rotate*  
    > Rotates the stack down by 1 position: the last element becomes the first.

- Guarantees:
  - no duplicate numbers;
  - all integers within the `int` (32-bit) range;
  - proper error handling and no memory leaks.

---

## Stack representation

The stacks are represented by a linked list from `libft`:

typedef struct s_data
{
    t_list  *a;
    t_list  *b;
}   t_data;

Main groups of functions:

-Parsing & validation

*parse_args – splits the arguments and builds stack A.

*is_str_valid – checks if a string represents a valid integer.

*safe_atoi – converts char * to int with overflow checking.

*is_doubled – checks for duplicate numbers.

*error_and_exit – frees memory, prints Error to stderr and exits the program.

Project operations:

*sa, sb, ss – swap the first two elements of a stack.

*pa, pb – move the top element of one stack to the other.

*ra, rb, rr – shift the stack up (first element goes to the bottom).

*rra, rrb, rrr – shift the stack down (last element goes to the top).

Sorting:

-Small cases

Functions: sort_two, sort_three, sort_four, sort_five.

Helpers: find_smallest, push_smallest_to_top, etc.

Idea: handle all small cases with minimal combinations of sa, ra, rra, pa, pb.

Indexing (coordinate compression):

Create an array with all values.

Sort the array.

Replace the value in each node by its index in the sorted array.

This reduces the problem to working with values from 0 to n - 1, which is much easier for algorithms like radix sort.

Medium cases (≤ 100 elements)

chunk_sort:

Split the indices into chunks (intervals).

Push to stack B the elements that belong to the current chunk using rotations.

Push everything back from B to A, always bringing the largest element from B to the top.

Large cases (> 100 elements)

radix:

Implements a binary radix sort on the indices.

For each bit:

traverse stack A;

push to B all elements whose current bit is 0;

then push everything back from B to A.

Repeat until all necessary bits have been processed, with complexity approximately O(n log n).

Repository structure

push_swap_from_42/
├── libft_applier/      # My implementation of libft (libft.a + headers)
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
The Makefile compiles libft inside libft_applier/ and then builds the push_swap binary.

Requirements
C compiler (cc, gcc or clang);

make;

Unix-like environment (Linux / macOS);

libft already included in libft_applier/
(no need to install it separately — the Makefile handles that).

Compilation
In the project root directory:

make        # builds libft_applier and the push_swap binary
make clean  # removes object files
make fclean # removes object files and the binary
make re     # full rebuild
▶️ Usage
Valid inputs
You can pass the numbers in two ways:

./push_swap 2 1 3 6 5 8
./push_swap "2 1 3 6 5 8"
If everything is correct, the program prints to standard output the sequence of operations, for example:

./push_swap 2 1 3
sa
In case of error (out-of-range integer, invalid argument, duplicate number, etc.), the program prints:

Error
to stderr and exits with status code 1.

Examples:
Count the number of operations

./push_swap 4 67 3 87 23 9 1 0 | wc -l
Generate random inputs and test with an external checker

# example: numbers from 0 to 99 and a list of 50 elements
ARG=$(seq 0 99 | shuf -n 50 | tr '\n' ' ')
./push_swap $ARG | ./checker_OS $ARG  # checks if the commands really sort the list
./push_swap $ARG | wc -l             # shows how many commands were used
(Replace checker_OS with whatever checker script/program you are using.)

🧪 Sorting strategies (summary)
2 to 5 elements
Specific cases using minimal combinations of sa, ra, rra, pa, pb.

Up to 100 elements
Value compression to indices (0..n-1);

Chunk division;

Push elements of each chunk to stack B;

Push back to A, always bringing the largest element from B to the top.

More than 100 elements
Binary radix sort:

for each bit, push to B all elements where that bit is 0;

at the end of each pass, push everything back to A;

repeat until all relevant bits have been processed.
