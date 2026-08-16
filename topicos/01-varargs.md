# 📌 Varargs (Variable Arguments) em Java

O recurso de **Varargs** (argumentos de comprimento variável), introduzido no Java 5, permite que um método receba **zero ou múltiplos argumentos** de um determinado tipo, sem a necessidade de criar e passar um array manualmente ou criar dezenas de sobrecargas de método.

---

## 🎯 Por que usar Varargs?

Antes do Varargs, se quiséssemos somar números flexivelmente, precisávamos:
1. Criar várias sobrecargas: `somar(int a, int b)`, `somar(int a, int b, int c)`, etc.
2. Ou exigir que quem chama o método passe um array: `somar(new int[]{1, 2, 3})`.

Com **Varargs**, podemos chamar o método com quantos valores quisermos: `somar(1, 2, 3, 4, 5)` ou até `somar()`.

---

## ⚙️ Sintaxe

Utiliza-se três pontos (`...`) após o tipo de dado:

```java
public void meuMetodo(Tipo... nomeDoParametro) {
    // nomeDoParametro é tratado como um array (Tipo[]) dentro do método
}
```

---

## ⚠️ Regras Obrigatórias

> [!IMPORTANT]
> 1. **Apenas um Varargs por método:** Não é permitido ter dois parâmetros com `...` no mesmo método.
> 2. **Deve ser o último parâmetro:** Se o método tiver outros parâmetros fixos, o varargs **obrigatoriamente** precisa ser o último da lista.

### ❌ Exemplos Inválidos (Erro de Compilação)

```java
// ERRO 1: Mais de um varargs no mesmo método
public void invalido(int... numeros, String... nomes) { }

// ERRO 2: Varargs não é o último parâmetro
public void invalido(int... numeros, String texto) { }
```

### ✅ Exemplo Válido

```java
// O parâmetro fixo vem antes, o varargs vem por último
public void valido(String prefixo, int... numeros) { }
```

---

## 💻 Exemplos Práticos

### 1. Soma de números inteiros (0 ou mais parâmetros)

```java
public class ExemploVarargsBasico {

    public static int somar(int... numeros) {
        int total = 0;
        // Por baixo dos panos, 'numeros' é um int[]
        for (int n : numeros) {
            total += n;
        }
        return total;
    }

    public static void main(String[] args) {
        System.out.println(somar());              // 0 (zero argumentos)
        System.out.println(somar(10));            // 10 (1 argumento)
        System.out.println(somar(1, 2, 3));       // 6 (3 argumentos)
        System.out.println(somar(5, 10, 15, 20)); // 50 (4 argumentos)
    }
}
```

---

### 2. Combinando Parâmetro Fixo com Varargs

```java
public class ExemploVarargsComParametroFixo {

    public static void exibirMensagens(String categoria, String... mensagens) {
        System.out.println("=== " + categoria.toUpperCase() + " ===");
        if (mensagens.length == 0) {
            System.out.println("(Nenhuma mensagem recebida)");
            return;
        }
        for (String msg : mensagens) {
            System.out.println("- " + msg);
        }
    }

    public static void main(String[] args) {
        exibirMensagens("Avisos"); // categoria fornecida, varargs vazio
        exibirMensagens("Logs de Erro", "Falha de rede", "Timeout no banco", "NullPointer");
    }
}
```

---

### 3. Passando um Array Existente para um Método Varargs

Um método que aceita `Tipo...` também aceita diretamente um array `Tipo[]`:

```java
public class ExemploArrayComVarargs {

    public static void imprimirNomes(String... nomes) {
        for (String nome : nomes) {
            System.out.println(nome);
        }
    }

    public static void main(String[] args) {
        String[] lista = {"Alice", "Bob", "Charlie"};
        
        // Passando diretamente o array
        imprimirNomes(lista);
    }
}
```

---

## 🔍 Como o Java trata o Varargs internamente?

Por baixo dos panos, o compilador Java:
1. Cria um array com os elementos passados na chamada: `new int[]{1, 2, 3}`.
2. Passa esse array para o método.
3. Trata o parâmetro dentro do método como `int[]`.

> [!NOTE]
> Como um novo array é alocado em memória a cada chamada, se o método for chamado milhões de vezes dentro de um loop crítico de performance, criar sobrecargas dedicadas para os casos mais comuns (ex: 1, 2 ou 3 parâmetros) pode evitar alocações desnecessárias no Garbage Collector.

---

## ⚠️ Cuidado com Sobrecarga (Overloading) Ambígua

Evite criar sobrecargas que possam gerar ambiguidade para o compilador:

```java
public class ExemploAmbiguidade {
    public static void teste(int... nums) { System.out.println("int..."); }
    public static void teste(Integer... nums) { System.out.println("Integer..."); }

    public static void main(String[] args) {
        // teste(1, 2); // ERRO DE COMPILAÇÃO: Chamada ambígua!
    }
}
```

---

## 🛡️ Varargs com Generics (`@SafeVarargs`)

Ao usar varargs com tipos genéricos (`T...`), o compilador pode emitir um aviso de *Heap Pollution*. Para suprimir esse aviso quando o método for seguro, use a anotação `@SafeVarargs`:

```java
import java.util.List;
import java.util.ArrayList;

public class ExemploSafeVarargs {

    @SafeVarargs
    public static <T> List<T> criarLista(T... elementos) {
        List<T> lista = new ArrayList<>();
        for (T elemento : elementos) {
            lista.add(elemento);
        }
        return lista;
    }
}
```

---

## 📋 Resumo Rápido

| Característica | Detalhe |
| :--- | :--- |
| **Sintaxe** | `Tipo... nome` |
| **Quantidade permitida** | Máximo **1** por método |
| **Posição** | Deve ser o **último** parâmetro |
| **Tratamento interno** | Array tradicional (`Tipo[]`) |
| **Argumentos aceitos** | Zero, um, múltiplos valores separados por vírgula ou um array pronto |

---

[⬅️ Voltar ao Índice Principal](../README.md)

