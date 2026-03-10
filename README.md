# Lista Linear (Sequencial) — **Exclusão de Elemento**

> Disciplina: Estrutura de Dados (C++)  
> Tema: Lista linear implementada com **array estático** (`int lista[MAX]` + `int nElementos`)  
> Objetivo: Implementar a função **`excluirElemento()`** utilizando a função **`posicaoElemento()`** para evitar duplicação de código. 

---

## 1) Contexto e Modelo de Dados

Nesta atividade, trabalharemos com uma **lista linear sequencial** (também chamada de *lista estática*), armazenada em um **array** de tamanho fixo `MAX` e controlada pela variável `nElementos`:

- `lista[MAX]`: armazena os valores inteiros;
- `nElementos`: quantidade atual de itens válidos (elementos ocupando as posições `0 ... nElementos-1`);
- O menu já dispõe de opções para **inserir**, **buscar**, **exibir** e **excluir**.

**Importante**:
1. Os elementos válidos estão sempre nas posições `0` até `nElementos - 1`.
2. Não deve haver "buracos" entre elementos — ao remover um item, **deslocamos** os seguintes uma posição à esquerda.
3. Nunca acessar índices `>= nElementos` (evita *lixo* ou leitura fora do limite).

---

## 2) Revisão conceitual (Arrays em C++)

- **Array estático**: tamanho definido em tempo de compilação (ex.: `const int MAX = 10; int lista[MAX]{};`).  
- **Acesso por índice**: `lista[i]` é **O(1)** (tempo constante).  
- **Inserção no fim**: se houver espaço, fazer `lista[nElementos] = valor; nElementos++` (**O(1)**).  
- **Busca linear**: varrer do índice `0` até `nElementos-1` comparando valores (**O(n)**).  
- **Exclusão**: localizar a posição **p** e deslocar os itens das posições `p+1 ... nElementos-1` para a esquerda (**O(n)**).

> Dica para o menu: `system("cls")` e `system("pause")` existem no Windows. Em outras plataformas, você pode comentar/remover essas linhas.

---

## 3) Passagem de Parâmetros e Retorno de Função em C++

Antes de implementar `posicaoElemento()`, é importante compreender como **parâmetros** e **retorno** funcionam em C++. Esse conhecimento é fundamental para escrever funções corretas e reutilizáveis.

### 3.1) Passagem de Parâmetros

Ao chamar uma função e passar um argumento, o C++ oferece duas modalidades principais:

| Modalidade | Sintaxe | O que acontece |
|---|---|---|
| **Por valor** | `void f(int x)` | Uma **cópia** do argumento é criada. Alterações em `x` dentro da função **não** afetam a variável original. |
| **Por referência** | `void f(int &x)` | A função recebe um **apelido** para a variável original. Alterações em `x` **afetam** o original. |

**Exemplo:**
```cpp
void dobraValor(int x) {     // por valor — não altera o original
    x = x * 2;
}

void dobraRef(int &x) {      // por referência — altera o original
    x = x * 2;
}

int main() {
    int a = 5;
    dobraValor(a);  // a continua 5
    dobraRef(a);    // a passa a ser 10
}
```

### 3.2) Retorno de Função

Uma função pode **devolver** um valor ao código que a chamou usando a palavra-chave `return`. O tipo do valor retornado deve coincidir com o tipo declarado na assinatura da função.

```cpp
int soma(int a, int b) {
    return a + b;   // devolve um int ao chamador
}

int resultado = soma(3, 4);  // resultado == 7
```

> **Atenção**: ao executar `return`, a função encerra imediatamente e o controle volta ao ponto da chamada. Qualquer código após o `return` **não** é executado.

### 3.3) Aplicando à `posicaoElemento(int valor)`

A função recebe **por valor** o inteiro `valor` que se deseja buscar — não precisa modificar a variável original, apenas consultá-la. Ela percorre o array `lista[]` e **retorna** o índice (`int`) da primeira ocorrência, ou `-1` caso o valor não exista:

```cpp
int posicaoElemento(int valor) {
    for (int i = 0; i < nElementos; i++) {
        if (lista[i] == valor) {
            return i;   // encontrou: devolve o índice imediatamente
        }
    }
    return -1;          // não encontrou: sinal de "ausente"
}
```

> **Por que retornar `int` e não `bool`?**
> Retornar o **índice** permite que o chamador saiba exatamente **onde** o elemento está — útil tanto para exibir a posição (`buscarElemento`) quanto para iniciar o deslocamento de remoção (`excluirElemento`), sem necessidade de uma segunda busca.

---

## 4) Evitando duplicação com `posicaoElemento(int valor)`

A busca por um valor aparece em vários pontos do programa (ex.: **buscar** e **excluir**). Para **não duplicar código**, utilizamos uma **função utilitária** que centraliza a busca:

### 4.1) Assinatura (já fornecida no projeto)
```cpp
int posicaoElemento(int valor);
```

### 4.2) Responsabilidade
- Percorrer as posições válidas (`0` a `nElementos-1`) da lista;
- **Retornar o índice** da **primeira ocorrência** do valor buscado;
- **Retornar `-1`** caso o valor **não** esteja presente na lista.

> **Por que retornar a primeira ocorrência?**  
> Mantém a função previsível e facilita operações como exclusão (remove o primeiro que encontrar).


---

## 5) Implementando `excluirElemento()`

- **Ler do usuário** o número a ser excluído;
- **Buscar** o número na lista usando **`posicaoElemento(valor)`**;
- **Se encontrado**: remover deslocando os elementos à esquerda; **decrementar `nElementos`**;
- **Se não encontrado**: exibir a mensagem **`"elemento não encontrado"`** 


---

## 6) Interações com o menu

- A função `excluirElemento()` deve ser chamada pela opção **6 – Excluir elemento** do menu.  
- A função `buscarElemento()` (opção 4) também deve usar **`posicaoElemento()`** — evite repetir um laço de busca ali.

Exemplo de uso dentro de `buscarElemento()` (já compatível com o projeto):
```cpp
void buscarElemento() {
    int valor;
    cout << "Digite o elemento que queira buscar: ";
    cin >> valor;

    int pos = posicaoElemento(valor);

    if (pos != -1) {
        cout << "O elemento foi encontrado na posicao " << pos << endl;
    } else {
        cout << "O elemento digitado nao foi encontrado" << endl;
    }
}
```

---

## 7) Casos de teste sugeridos

1. **Excluir em lista vazia**: opção 6 sem inserir nada → deve avisar que está vazia.  
2. **Excluir elemento inexistente**: insira `[3, 7, 9]`, tente excluir `5` → `"elemento nao encontrado"`.  
3. **Excluir primeiro elemento**: de `[3, 7, 9]` exclua `3` → resultado `[7, 9]`, `nElementos = 2`.  
4. **Excluir último elemento**: de `[7, 9]` exclua `9` → resultado `[7]`, `nElementos = 1`.  
5. **Excluir elemento do meio**: de `[2, 5, 8, 10]` exclua `8` → resultado `[2, 5, 10]`, `nElementos = 3`.  
6. **Inserir duplicados (se permitido)**: se decidir permitir duplicados, exclua `5` em `[5, 5, 5]` → deve restar `[5, 5]`.  
   - *Observação*: o código acima remove **apenas a primeira ocorrência**. Para remover **todas**, repita exclusões enquanto `posicaoElemento(valor) != -1`.

---

## 8) Referências Complementares

### Livros

- **Deitel, P. & Deitel, H.** — *C++: Como Programar* (14ª ed.). Pearson. Os capítulos sobre funções (Cap. 6) e arrays (Cap. 7) cobrem passagem de parâmetros e manipulação de vetores com profundidade.
- **Ziviani, N.** — *Projeto de Algoritmos com Implementações em Pascal e C* (3ª ed.). Cengage. O Capítulo 3 trata especificamente de listas lineares sequenciais e encadeadas.
- **Cormen, T. H. et al.** — *Introdução a Algoritmos* (3ª ed.). MIT Press / GEN-LTC. Referência clássica para análise de estruturas de dados e algoritmos.

### Sites e Documentação

- [cppreference.com — Functions](https://en.cppreference.com/w/cpp/language/functions): documentação completa sobre declaração, passagem de parâmetros e tipos de retorno em C++.
- [cplusplus.com — Arrays](https://cplusplus.com/doc/tutorial/arrays/): tutorial sobre arrays unidimensionais em C++.
- [GeeksforGeeks — Linear Search](https://www.geeksforgeeks.org/linear-search/): explicação e exemplos de busca linear com análise passo a passo.
- [Visualgo.net — List](https://visualgo.net/en/list): visualização animada de operações em listas lineares (inserção, remoção, busca).



## 9) Checklist para entrega

- [ ] `excluirElemento()` implementada conforme especificação.  
- [ ] `posicaoElemento()` retorna **a primeira ocorrência** e é **utilizada** por `excluirElemento()` e `buscarElemento()`.  
- [ ] Mensagens de saída conforme exemplos (atenção à ortografia simples sem acentos, se preferir manter padrão do projeto).  
- [ ] Testes executados (incluindo casos de sucesso e erro).  
- [ ] Código comentado explicando as decisões principais.

---

Boa prática e bons estudos! ✨

