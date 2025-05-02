# Solucionador de Programação Inteira Binária via Branch and Bound

Este projeto implementa o algoritmo Branch and Bound (B&B) para resolver problemas de programação linear inteira binária (variáveis só podem ser 0 ou 1) de maximização. O código utiliza a biblioteca `mip` (Python-MIP) para resolver os problemas lineares relaxados em cada nó da árvore de B&B.

## 🎯 Funcionalidades

* Resolve problemas de programação linear inteira binária (0-1) com o objetivo de maximização.
* Implementa a estratégia Branch and Bound clássica.
* Permite a entrada de dados via console ou através de um arquivo de texto.
* Utiliza o solver CBC (Coin-or Branch and Cut) através da biblioteca `mip`.
* Identifica a variável mais próxima de 0.5 para realizar a ramificação (branching).

## 📦 Dependências

Para executar este código, você precisará ter as seguintes bibliotecas Python instaladas:

* **mip**: `pip install mip`
* **numpy**: `pip install numpy`

## ⚙️ Como Executar

1.  **Clone ou baixe** o arquivo `B&B.py`.
2.  **Instale as dependências** listadas acima.
3.  **Execute o script** Python pelo terminal:
    ```bash
    python B&B.py
    ```
4.  **Escolha o método de entrada**:
    * Digite `1` para inserir os dados manualmente pelo console.
    * Digite `2` para ler os dados de um arquivo.

### Entrada via Console (Método 1)

Se escolher o método 1, o programa solicitará:

1.  **Número de variáveis e restrições**: Insira dois números inteiros separados por espaço (e.g., `3 2` para 3 variáveis e 2 restrições).
2.  **Coeficientes da função objetivo**: Insira os coeficientes de cada variável na função objetivo, separados por espaço (e.g., `5 4 6`).
3.  **Coeficientes e lado direito das restrições**: Para cada restrição, insira os coeficientes das variáveis e o valor do lado direito (termo independente), todos separados por espaço. O último número da linha é o lado direito.
    * Exemplo para a primeira restrição: `1 2 1 10` (significa `1*x1 + 2*x2 + 1*x3 <= 10`)
    * Exemplo para a segunda restrição: `3 1 2 15` (significa `3*x1 + 1*x2 + 2*x3 <= 15`)

### Entrada via Arquivo (Método 2)

Se escolher o método 2:

1.  **Nome do arquivo**: Digite o nome do arquivo de texto contendo os dados do problema (e.g., `problema.txt`).
2.  **Formato do arquivo**: O arquivo deve seguir o seguinte formato:
    * **Linha 1**: Número de variáveis e número de restrições, separados por espaço.
    * **Linha 2**: Coeficientes da função objetivo, separados por espaço.
    * **Linhas seguintes**: Cada linha representa uma restrição, contendo os coeficientes das variáveis e o valor do lado direito, separados por espaço. O último número é o lado direito.

    *Exemplo de arquivo (`problema.txt`):*
    ```
    3 2
    5 4 6
    1 2 1 10
    3 1 2 15
    ```

## 🌳 Algoritmo Branch and Bound Implementado

1.  **Inicialização**: O problema original (relaxado, com variáveis contínuas entre 0 e 1) é adicionado a uma fila. O limite primal (melhor solução inteira encontrada até agora) é inicializado como 0.
2.  **Loop Principal**: Enquanto a fila não estiver vazia:
    * **Seleção**: Um nó (subproblema) é removido da fila.
    * **Bound (Limitação)**: O problema linear relaxado do nó é resolvido usando o solver CBC.
        * **Poda por Inviabilidade**: Se o subproblema for inviável, o nó é descartado.
        * **Poda por Limite**: Se a solução ótima do subproblema relaxado for pior (menor ou igual) que o limite primal atual, o nó é descartado (não pode levar a uma solução inteira melhor).
        * **Poda por Integralidade**: Se a solução do subproblema relaxado for inteira (todas as variáveis 0 ou 1):
            * Se for melhor que o limite primal atual, atualiza o limite primal e armazena essa como a melhor solução encontrada até agora.
            * O nó é descartado (não precisa mais ramificar).
    * **Branch (Ramificação)**: Se a solução do subproblema relaxado for fracionária (pelo menos uma variável não é 0 nem 1):
        * **Seleção da Variável**: Uma variável fracionária é selecionada para ramificação (neste código, a que tiver o valor mais próximo de 0.5).
        * **Criação de Subproblemas**: Dois novos nós (subproblemas) são criados, adicionando uma nova restrição a cada um:
            * Nó 1: Variável selecionada = 0
            * Nó 2: Variável selecionada = 1
        * Os novos nós são adicionados à fila.
3.  **Término**: O algoritmo termina quando a fila está vazia. A melhor solução inteira armazenada é a solução ótima do problema original.

## 📊 Saída

Após a execução, o programa imprimirá:

* Os dados de entrada lidos.
* Mensagens indicando o progresso e quando soluções inteiras são encontradas.
* A **melhor solução inteira encontrada**:
    * O valor ótimo da função objetivo.
    * Os valores (0 ou 1) das variáveis na solução ótima.

