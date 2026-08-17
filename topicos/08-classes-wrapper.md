# 🎁 Classes Wrapper (Wrapper Classes) em Java

As **Classes Wrapper** (*classes invólucro / envelope*) pertencem ao pacote `java.lang` e fornecem uma forma de tratar os **8 tipos primitivos** do Java como **Objetos** de referência.

Elas encapsulam o valor primitivo dentro de um objeto e disponibilizam uma vasta coleção de **métodos utilitários** para conversão, formatação e manipulação.

---

## 🎯 Por que precisamos de Wrapper Classes?

1. **Coleções e Generics:** Estruturas de dados como `ArrayList`, `HashSet` e `HashMap` trabalham **exclusivamente com Objetos**. Não é possível fazer `List<int>`, sendo obrigatório o uso de `List<Integer>`.
2. **Métodos Utilitários:** Conversões de texto para números (`Integer.parseInt()`), checagem de caracteres (`Character.isDigit()`), conversão para binário/hexadecimal, etc.
3. **Representação de Valores Nulos (`null`):** Um primitivo `int` sempre tem valor padrão (`0`), enquanto um `Integer` pode ser `null` (fundamental ao lidar com bancos de dados e APIs REST).
4. **Polimorfismo e `Object`:** Permite que primitivos sejam passados para métodos que aceitam qualquer tipo de objeto (`Object`).

---

## 📊 Tabela de Tipos Primitivos vs Wrapper Classes

| Tipo Primitivo | Wrapper Class | Classe Mãe Direta | Valor Padrão (Primitivo vs Objeto) |
| :--- | :--- | :--- | :---: |
| `byte` | `Byte` | `java.lang.Number` | `0` vs `null` |
| `short` | `Short` | `java.lang.Number` | `0` vs `null` |
| `int` | `Integer` | `java.lang.Number` | `0` vs `null` |
| `long` | `Long` | `java.lang.Number` | `0L` vs `null` |
| `float` | `Float` | `java.lang.Number` | `0.0f` vs `null` |
| `double` | `Double` | `java.lang.Number` | `0.0d` vs `null` |
| `char` | `Character` | `java.lang.Object` | `'\u0000'` vs `null` |
| `boolean` | `Boolean` | `java.lang.Object` | `false` vs `null` |

---

## 🔄 Autoboxing e Unboxing

Introduzidos no Java 5, facilitam a conversão automática e transparente entre primitivos e seus respectivos objetos wrapper.

### 1. Autoboxing (Primitivo $\rightarrow$ Objeto Wrapper)
O compilador Java converte automaticamente um tipo primitivo em sua classe wrapper correspondente:

```java
// O que você escreve:
Integer numObjeto = 25; 

// O que o compilador faz por baixo dos panos:
Integer numObjeto = Integer.valueOf(25);
```

### 2. Unboxing (Objeto Wrapper $\rightarrow$ Primitivo)
O compilador converte automaticamente o objeto wrapper de volta para o valor primitivo:

```java
Integer valorWrapper = 50;

// O que você escreve:
int valorPrimitivo = valorWrapper; 

// O que o compilador faz por baixo dos panos:
int valorPrimitivo = valorWrapper.intValue();
```

---

## 💻 Métodos Utilitários Mais Utilizados

### 1. Conversão de Strings para Números (*Parsing*)

```java
public class ExemploParsing {
    public static void main(String[] args) {
        String textoInt = "123";
        String textoDouble = "45.67";
        String textoBool = "true";

        int numero = Integer.parseInt(textoInt);           // 123
        double decimal = Double.parseDouble(textoDouble);   // 45.67
        boolean ativo = Boolean.parseBoolean(textoBool);    // true

        System.out.println("Soma: " + (numero + 10));      // 133
    }
}
```

---

### 2. Métodos da Classe `Character`

```java
public class ExemploCharacter {
    public static void main(String[] args) {
        char letra = 'a';
        char numero = '7';
        char espaco = ' ';

        System.out.println(Character.isLetter(letra));      // true
        System.out.println(Character.isDigit(numero));      // true
        System.out.println(Character.isWhitespace(espaco)); // true
        System.out.println(Character.toUpperCase(letra));   // 'A'
    }
}
```

---

### 3. Constantes de Limites e Utilitários Numéricos

```java
public class ExemploLimites {
    public static void main(String[] args) {
        System.out.println("Maior int: " + Integer.MAX_VALUE); // 2147483647
        System.out.println("Menor int: " + Integer.MIN_VALUE); // -2147483648

        // Conversões de base numérica:
        System.out.println("10 em Binário: " + Integer.toBinaryString(10)); // "1010"
        System.out.println("255 em Hexadecimal: " + Integer.toHexString(255)); // "ff"
    }
}
```

---

## ⚠️ Armadilhas e Cuidados Críticos

### 1. Comparação de Objetos com `==` e o *Integer Cache*

> [!CAUTION]
> Para comparar o valor de duas classes wrapper, **SEMPRE use `.equals()`**, nunca `==`.

O Java mantém um cache interno para valores de `Byte`, `Short`, `Integer` e `Long` no intervalo de **-128 a 127**:

```java
Integer a = 100;
Integer b = 100;
System.out.println(a == b);      // true (dentro do cache -128 a 127)

Integer c = 200;
Integer d = 200;
System.out.println(c == d);      // ❌ FALSE! (Fora do cache, são referências de memória diferentes)
System.out.println(c.equals(d)); // ✅ TRUE (Compara o conteúdo)
```

---

### 2. `NullPointerException` durante o Unboxing

Se uma variável wrapper estiver nula e você tentar atribuí-la a um primitivo ou usá-la em operações matemáticas, uma exceção será lançada:

```java
Integer saldo = null;

// ❌ Lança NullPointerException em tempo de execução:
// int saldoPrimitivo = saldo; 

// ✅ Verificação segura antes do unboxing:
if (saldo != null) {
    int saldoPrimitivo = saldo;
    System.out.println(saldoPrimitivo);
}
```

---

### 3. Custo de Desempenho (*Performance Overhead*) em Loops

Criar objetos wrapper dentro de loops pesados gera milhares de alocações na memória Heap e sobrecarrega o *Garbage Collector*:

```java
// ❌ LENTO: Autoboxing a cada iteração cria bilhões de objetos Long na memória
Long somaObjeto = 0L;
for (long i = 0; i < 1_000_000; i++) {
    somaObjeto += i; // Desempacota, soma e empacota um novo objeto Long
}

// ✅ RÁPIDO: Primitivo trabalha diretamente nos registradores/Stack
long somaPrimitiva = 0L;
for (long i = 0; i < 1_000_000; i++) {
    somaPrimitiva += i;
}
```

---

## 📋 Resumo Rápido

| Conceito | Descrição |
| :--- | :--- |
| **Definição** | Versões Orientadas a Objetos dos tipos primitivos (`int` $\rightarrow$ `Integer`) |
| **Autoboxing** | Conversão automática do primitivo para wrapper (`Integer x = 10;`) |
| **Unboxing** | Conversão automática do wrapper para primitivo (`int y = x;`) |
| **Coleções** | Obrigatórias em Generics (`List<Double>`, `Map<String, Integer>`) |
| **Comparação** | Sempre comparar com `.equals()`, nunca com `==` |
| **Cuidado** | Cuidado com `NullPointerException` no unboxing de variáveis nulas |

---

[⬅️ Voltar ao Índice Principal](../README.md)
