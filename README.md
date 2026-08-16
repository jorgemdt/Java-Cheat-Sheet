# ☕ Java Cheat Sheet

Um guia prático, direto ao ponto e estruturado para consulta rápida de sintaxe, conceitos fundamentais e recursos avançados da linguagem **Java**.

## 💡 Como Utilizar

- Cada item do índice abaixo contém um **resumo direto** com **código de exemplo**.
- Clique no link de cada tópico para acessar a **explicação detalhada e completa**.

---

## 📌 Índice de Tópicos


### 00. [Fundamentos Básicos da Linguagem](topicos/00-fundamentos-basicos.md)
Sintaxe essencial e conceitos universais de programação implementados no Java:

- **Variáveis & Tipos:** `int`, `double`, `boolean`, `char`, `String` e inferência local `var`.
- **Entrada e Saída:** `System.out.println()`, `System.out.printf()` e `Scanner`.
- **Operadores & Condicionais:** Aritméticos, lógicos, ternário, `if-else` e `switch`.
- **Loops & Arrays:** `for`, `while`, `for-each` e vetores indexados `tipo[]`.

```java
// Variáveis e Impressão formatada
String nome = "Dev";
int idade = 20;
System.out.printf("Nome: %s, Idade: %d\n", nome, idade);

// Condicional com Operador Ternário
String status = (idade >= 18) ? "Maior de idade" : "Menor de idade";

// Array e laço for-each
int[] numeros = {10, 20, 30};
for (int n : numeros) {
    System.out.println("Item: " + n);
}
```

[⬆️ Voltar ao Início](#-java-cheat-sheet)

---

### 01. [Varargs (Variable Arguments)](topicos/01-varargs.md)
Permite que um método receba **zero ou múltiplos argumentos** de um mesmo tipo (`Tipo...`), sendo convertidos internamente em um array.

> ⚠️ **Regra:** Deve ser **sempre o último parâmetro** e só pode haver **um** varargs por método.

```java
public static int somar(int... numeros) {
    int total = 0;
    for (int n : numeros) {
        total += n;
    }
    return total;
}

// Exemplos de uso:
somar();            // Retorna 0
somar(10);          // Retorna 10
somar(1, 2, 3, 4);  // Retorna 10
```

[⬆️ Voltar ao Início](#-java-cheat-sheet)

---
