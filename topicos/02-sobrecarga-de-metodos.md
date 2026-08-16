# 🔀 Sobrecarga de Métodos (Method Overloading) em Java

A **Sobrecarga de Métodos** (*Method Overloading*) ocorre quando dois ou mais métodos na mesma classe compartilham o **mesmo nome**, mas possuem **listas de parâmetros diferentes** (quantidade, tipos ou ordem dos tipos).

É uma forma de **polimorfismo em tempo de compilação** (polimorfismo estático).

---

## 🎯 Por que usar Sobrecarga?

Permite que o mesmo conceito/operação seja executado com diferentes entradas sem a necessidade de criar nomes artificiais como `somarDoisNumeros()`, `somarTresNumeros()`, `somarDecimais()`. Usamos apenas `somar()`.

---

## 📐 O que compõe a Assinatura de um Método?

Em Java, o compilador diferencia métodos exclusivamente pela sua **assinatura**:

$$\text{Assinatura} = \text{Nome do Método} + \text{Lista de Tipos de Parâmetros}$$

> [!IMPORTANT]
> O **tipo de retorno NÃO faz parte da assinatura**. Mudar apenas o tipo de retorno **NÃO** é sobrecarga e resulta em erro de compilação!

```java
// ❌ ERRO DE COMPILAÇÃO: O compilador não sabe qual executar se apenas o retorno mudar
public int calcular(int a) { return a * 2; }
public double calcular(int a) { return a * 2.0; } // Erro: método duplicado!
```

---

## 🛠️ Formas de Realizar a Sobrecarga

Para ser uma sobrecarga válida, a lista de parâmetros deve variar em pelo menos um destes 3 aspectos:

### 1. Variação na **Quantidade** de Parâmetros

```java
public class Calculadora {

    public int somar(int a, int b) {
        return a + b;
    }

    // Sobrecarga com 3 parâmetros
    public int somar(int a, int b, int c) {
        return a + b + c;
    }
}
```

---

### 2. Variação nos **Tipos** de Parâmetros

```java
public class Impressora {

    public void imprimir(String texto) {
        System.out.println("Texto: " + texto);
    }

    // Sobrecarga com int
    public void imprimir(int numero) {
        System.out.println("Número inteiro: " + numero);
    }

    // Sobrecarga com double
    public void imprimir(double decimal) {
        System.out.println("Decimal: " + decimal);
    }
}
```

---

### 3. Variação na **Ordem** dos Tipos de Parâmetros

```java
public class Exibidor {

    public void exibir(String tag, int id) {
        System.out.println(tag + ": " + id);
    }

    // Sobrecarga invertendo os tipos (int antes de String)
    public void exibir(int id, String tag) {
        System.out.println(id + " - " + tag);
    }
}
```

---

## 🏗️ Sobrecarga de Construtores (*Constructor Overloading*)

Assim como métodos comuns, **construtores** também podem ser sobrecarregados para permitir criar objetos de maneiras diferentes (com valores padrão, parciais ou completos).

Podemos reutilizar construtores utilizando `this(...)`:

```java
public class Usuario {
    private String nome;
    private String email;
    private int idade;

    // Construtor 1: Completo
    public Usuario(String nome, String email, int idade) {
        this.nome = nome;
        this.email = email;
        this.idade = idade;
    }

    // Construtor 2: Sem idade (define padrão 0 chamando o construtor 1)
    public Usuario(String nome, String email) {
        this(nome, email, 0); // Chama o outro construtor
    }

    // Construtor 3: Padrão / Vazio
    public Usuario() {
        this("Anônimo", "sem-email@teste.com", 0);
    }
}
```

---

## 🔍 Resolução do Compilador (Type Promotion & Prioridades)

Quando um método sobrecarregado é chamado, o compilador Java escolhe a versão mais específica seguindo a seguinte ordem de prioridade:

1. **Correspondência Exata:** Tipo exatamente igual.
2. **Promoção de Tipo Primitivo (*Widening*):** `byte` $\rightarrow$ `short` $\rightarrow$ `int` $\rightarrow$ `long` $\rightarrow$ `float` $\rightarrow$ `double`.
3. **Autoboxing / Unboxing:** `int` $\rightarrow$ `Integer`.
4. **Varargs:** `int...` (é sempre a **última prioridade** na escolha).

### Exemplo de Type Promotion:

```java
public class TestePromocao {
    public static void processar(int n) {
        System.out.println("int: " + n);
    }

    public static void processar(double d) {
        System.out.println("double: " + d);
    }

    public static void main(String[] args) {
        short x = 10;
        processar(x); // Executa processar(int) porque short é promovido para int antes de double!
    }
}
```

---

## ⚠️ Armadilha: Chamada Ambígua (*Ambiguous Method Call*)

Cuidado ao misturar promoção de tipos e múltiplos argumentos:

```java
public class Ambiguidade {
    public static void demo(int a, double b) { }
    public static void demo(double a, int b) { }

    public static void main(String[] args) {
        // demo(10, 20); // ERRO DE COMPILAÇÃO: Chamada ambígua!
        // O Java não sabe se promove o 1º ou o 2º int para double.
    }
}
```

---

## 📋 Resumo Rápido

| Aspecto | É permitido para sobrecarga? |
| :--- | :--- |
| Mudar a **quantidade** de parâmetros | ✅ Sim |
| Mudar os **tipos** de parâmetros | ✅ Sim |
| Mudar a **ordem** dos tipos de parâmetros | ✅ Sim |
| Mudar apenas o **nome dos parâmetros** | ❌ Não (Erro de compilação) |
| Mudar apenas o **tipo de retorno** | ❌ Não (Erro de compilação) |
| Mudar apenas o **modificador de acesso** (`public`, `private`) | ❌ Não (Erro de compilação) |

---

[⬅️ Voltar ao Índice Principal](../README.md)
