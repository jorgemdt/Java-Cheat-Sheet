# 📋 ArrayList em Java (Listas Dinâmicas)

O **`ArrayList`** (do pacote `java.util`) é uma das estruturas de dados mais utilizadas em Java. Ele representa um **array dinâmico e redimensionável**, que cresce ou diminui automaticamente conforme elementos são adicionados ou removidos.

O `ArrayList` implementa a interface `List<E>` e faz parte do *Java Collections Framework*.

---

## 🧭 Array Estático (`tipo[]`) vs `ArrayList<E>`

| Característica | Array Tradicional (`tipo[]`) | `ArrayList<E>` |
| :--- | :--- | :--- |
| **Tamanho** | Fixo (definido na criação) | **Dinâmico** (redimensiona automaticamente) |
| **Tipos aceitos** | Primitivos (`int[]`) e Objetos (`String[]`) | **Apenas Objetos** (usa Wrappers como `Integer`) |
| **Inserção** | `arr[0] = valor;` | `lista.add(valor);` |
| **Acesso** | `arr[i]` | `lista.get(i)` |
| **Modificação** | `arr[i] = novoValor;` | `lista.set(i, novoValor);` |
| **Remoção** | Difícil (requer deslocar elementos manualmente) | Nativa com `lista.remove(i)` |
| **Tamanho atual** | `.length` (atributo) | `.size()` (método) |
| **Impressão** | Requer `Arrays.toString()` | Nativa e legível com `System.out.println(lista)` |

---

## ⚙️ Declaração e Inicialização

> [!TIP]
> **Boa Prática:** Sempre declare a variável utilizando a interface genérica `List<T>` à esquerda, instanciando `ArrayList<T>` à direita:

```java
import java.util.ArrayList;
import java.util.List;

// Lista vazia pronta para uso
List<String> frutas = new ArrayList<>();

// Inicialização com valores prévios (imutável)
List<String> imutavel = List.of("Maçã", "Banana", "Uva");

// Inicialização mutável a partir de outra lista
List<String> mutavel = new ArrayList<>(List.of("Maçã", "Banana", "Uva"));
```

---

## 🛠️ Métodos Principais (Operações CRUD)

```java
import java.util.ArrayList;
import java.util.List;

public class OperacoesArrayList {
    public static void main(String[] args) {
        List<String> linguagens = new ArrayList<>();

        // 1. ADICIONAR (Create)
        linguagens.add("Java");            // Adiciona ao final
        linguagens.add("Python");
        linguagens.add("C++");
        linguagens.add(1, "JavaScript");   // Insere no índice 1 (desloca os demais)

        System.out.println("Lista: " + linguagens); 
        // [Java, JavaScript, Python, C++]

        // 2. ACESSAR (Read)
        String primeiro = linguagens.get(0); // "Java"
        int total = linguagens.size();       // 4

        // 3. ATUALIZAR (Update)
        linguagens.set(2, "TypeScript");     // Substitui o item do índice 2

        // 4. REMOVER (Delete)
        linguagens.remove(0);                // Remove por índice ("Java")
        linguagens.remove("C++");            // Remove por objeto/valor

        // 5. CONSULTAS E VERIFICAÇÕES
        boolean contem = linguagens.contains("TypeScript"); // true
        int indice = linguagens.indexOf("Python");          // -1 (não encontrado)
        boolean vazia = linguagens.isEmpty();               // false

        // 6. LIMPAR TODOS OS ELEMENTOS
        linguagens.clear();
        System.out.println("Está vazia? " + linguagens.isEmpty()); // true
    }
}
```

---

## 🔁 Formas de Percorrer (Iterar) um ArrayList

### 1. Loop `for-each` (Mais legível e comum)
```java
List<String> nomes = List.of("Alice", "Bob", "Charlie");
for (String nome : nomes) {
    System.out.println(nome);
}
```

### 2. Loop `for` tradicional (Quando o índice é necessário)
```java
for (int i = 0; i < nomes.size(); i++) {
    System.out.println(i + ": " + nomes.get(i));
}
```

### 3. `forEach` funcional com Expressão Lambda (Java 8+)
```java
nomes.forEach(nome -> System.out.println("Item: " + nome));
// Ou com Method Reference:
nomes.forEach(System.out::println);
```

---

## 🔀 Ordenação de ArrayList

Podemos ordenar usando `Collections.sort()` ou o método `.sort()` nativo com `Comparator`:

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class OrdenacaoArrayList {
    public static void main(String[] args) {
        List<Integer> numeros = new ArrayList<>(List.of(40, 10, 50, 20, 30));

        // Ordem Crescente
        Collections.sort(numeros);
        System.out.println("Crescente: " + numeros); // [10, 20, 30, 40, 50]

        // Ordem Decrescente
        numeros.sort(Comparator.reverseOrder());
        System.out.println("Decrescente: " + numeros); // [50, 40, 30, 20, 10]
    }
}
```

---

## 🧺 ArrayList 2D (Lista de Listas)

Podemos aninhar `ArrayList` dentro de outro `ArrayList` para criar matrizes dinâmicas multidimensionais (ex: lista de compras por categoria):

```java
import java.util.ArrayList;

public class ExemploArrayList2D {
    public static void main(String[] args) {
        // Listas individuais
        ArrayList<String> padaria = new ArrayList<>(List.of("Pão", "Bolo", "Rosca"));
        ArrayList<String> hortifruti = new ArrayList<>(List.of("Tomate", "Alface", "Maçã"));
        ArrayList<String> bebidas = new ArrayList<>(List.of("Café", "Suco", "Refrigerante"));

        // Lista 2D contendo as outras listas
        ArrayList<ArrayList<String>> listaDeCompras = new ArrayList<>();
        listaDeCompras.add(padaria);
        listaDeCompras.add(hortifruti);
        listaDeCompras.add(bebidas);

        // Acessando um item específico (Categoria 0 -> Item 1 = "Bolo")
        System.out.println("Item escolhido: " + listaDeCompras.get(0).get(1)); // Bolo

        // Percorrendo a matriz 2D
        for (ArrayList<String> categoria : listaDeCompras) {
            System.out.println(categoria);
        }
    }
}
```

---

## ⚠️ Armadilhas e Cuidados Críticos

### 1. Remoção em `List<Integer>`: Índice vs Objeto

> [!CAUTION]
> Ao usar `List<Integer>`, chamar `remove(2)` remove o **elemento no índice 2**, e **NÃO** o número `2`!

```java
List<Integer> nums = new ArrayList<>(List.of(10, 20, 2, 40));

nums.remove(2);                   // Remove o elemento no ÍNDICE 2 (número 2)
nums.remove(Integer.valueOf(20)); // Remove o OBJETO com valor 20
```

---

### 2. `ConcurrentModificationException` ao remover em loops

Nunca remova elementos de uma lista usando um `for-each` comum:

```java
List<String> itens = new ArrayList<>(List.of("A", "B", "C"));

// ❌ LANÇA ConcurrentModificationException:
// for (String item : itens) {
//     if (item.equals("B")) itens.remove(item);
// }

// ✅ FORMA CORRETA 1: Usando removeIf (Java 8+)
itens.removeIf(item -> item.equals("B"));

// ✅ FORMA CORRETA 2: Usando Iterator
// Iterator<String> it = itens.iterator();
// while (it.hasNext()) { if (it.next().equals("B")) it.remove(); }
```

---

### 3. Listas Imutáveis com `List.of()` e `Arrays.asList()`

Listas criadas com `List.of(...)` são **100% imutáveis**. Tentar chamar `.add()` ou `.remove()` nelas lançará `UnsupportedOperationException`:

```java
List<String> fixa = List.of("A", "B");
// fixa.add("C"); // ❌ Erro: UnsupportedOperationException

// Para torná-la mutável, passe-a para o construtor do ArrayList:
List<String> mutavel = new ArrayList<>(fixa);
mutavel.add("C"); // ✅ Funciona perfeitamente
```

---

## 📋 Resumo Rápido

| Método | Ação |
| :--- | :--- |
| `add(e)` / `add(i, e)` | Insere elemento no fim ou em posição específica |
| `get(i)` | Obtém o elemento do índice `i` |
| `set(i, e)` | Substitui o elemento do índice `i` por `e` |
| `remove(i)` / `remove(obj)` | Remove por índice ou por valor correspondente |
| `size()` | Retorna a quantidade de elementos atuais |
| `contains(obj)` | Verifica se o elemento existe na lista (`true`/`false`) |
| `clear()` | Remove todos os elementos da lista |
| `removeIf(predicado)` | Remove elementos que atendem a uma condição lambda |

---

[⬅️ Voltar ao Índice Principal](../README.md)
