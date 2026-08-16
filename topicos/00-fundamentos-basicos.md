# 🧱 Fundamentos Básicos em Java

Conceitos universais de programação (presentes em praticamente qualquer linguagem como Python, JavaScript, C++, C#), com sua sintaxe e regras de implementação específicas em **Java**.

---

## 1. 🏁 Estrutura Básica e Hello World

Java é uma linguagem orientada a objetos compilada para *bytecode*. Todo código precisa estar dentro de uma classe, e a execução sempre começa no método `main`.

```java
public class OlaMundo {
    // Ponto de entrada (entry point) da aplicação
    public static void main(String[] args) {
        System.out.println("Olá, Mundo!"); // Imprime com quebra de linha
        System.out.print("Texto sem quebra"); // Imprime sem pular linha
    }
}
```

---

## 2. 📦 Variáveis e Tipos de Dados

Java é uma linguagem **estaticamente tipada** (o tipo da variável deve ser declarado e não pode mudar).

### Tipos Primitivos mais comuns:
```java
// Inteiros
byte idadePequena = 127;          // 8 bits (-128 a 127)
short ano = 2026;                 // 16 bits
int idade = 25;                   // 32 bits (Padrão para números inteiros)
long populacaoMundial = 8000000000L; // 64 bits (necessita do sufixo 'L')

// Decimais / Ponto Flutuante
float preco = 19.99f;             // 32 bits (necessita do sufixo 'f')
double salario = 4500.50;         // 64 bits (Padrão para casas decimais)

// Caractere e Booleano
char genero = 'M';                // Aspas simples para único caractere
boolean ativo = true;             // true ou false

// Tipo de Referência (Objeto)
String nome = "Maria";            // Aspas duplas para texto
```

> [!TIP]
> Desde o Java 10, você pode usar inferência de tipo local com `var`:
> ```java
> var contador = 10;      // O compilador infere como int
> var mensagem = "Olá";   // O compilador infere como String
> ```

---

## 3. ⌨️ Entrada e Saída de Dados (I/O)

Para ler dados do terminal, utiliza-se a classe `java.util.Scanner`.

```java
import java.util.Scanner;

public class EntradaSaida {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Digite seu nome: ");
        String nome = scanner.nextLine(); // Lê uma linha de texto

        System.out.print("Digite sua idade: ");
        int idade = scanner.nextInt();     // Lê um número inteiro

        System.out.print("Digite seu peso: ");
        double peso = scanner.nextDouble(); // Lê um decimal

        // Saída formatada com printf
        System.out.printf("Olá %s! Você tem %d anos e pesa %.2f kg.\n", nome, idade, peso);

        scanner.close(); // Boa prática: fechar o scanner
    }
}
```

> [!WARNING]
> **Armadilha comum do Scanner:** Ao ler um número (`nextInt()`) antes de ler um texto (`nextLine()`), a quebra de linha `\n` fica pendente no buffer. Limpe o buffer chamando `scanner.nextLine()` intermediário.

---

## 4. ➗ Operadores

### Aritméticos:
```java
int a = 10, b = 3;
int soma        = a + b;  // 13
int subtracao   = a - b;  // 7
int produto     = a * b;  // 30
int divisaoInt  = a / b;  // 3 (divisão de inteiros trunca o decimal)
double divisao  = (double) a / b; // 3.3333... (com casting)
int resto       = a % b;  // 1 (módulo / resto da divisão)
```

### Relacionais (Comparação):
```java
boolean igual        = (a == b); // false
boolean diferente    = (a != b); // true
boolean maior        = (a > b);  // true
boolean menorOuIgual = (a <= b); // false
```

> [!CAUTION]
> Para comparar **Strings** ou outros objetos, **nunca use `==`** (que compara o endereço de memória). Use `.equals()`:
> ```java
> String s1 = new String("teste");
> String s2 = "teste";
> boolean correto = s1.equals(s2); // true
> ```

### Lógicos:
```java
boolean c1 = true, c2 = false;
boolean eLogico   = c1 && c2; // false (AND - E)
boolean ouLogico  = c1 || c2; // true  (OR - OU)
boolean negacao   = !c1;      // false (NOT - NÃO)
```

### Operador Ternário:
```java
int nota = 7;
String resultado = (nota >= 6) ? "Aprovado" : "Reprovado";
```

---

## 5. 🔀 Estruturas Condicionais

### `if`, `else if` e `else`:
```java
int hora = 14;

if (hora < 12) {
    System.out.println("Bom dia!");
} else if (hora < 18) {
    System.out.println("Boa tarde!");
} else {
    System.out.println("Boa noite!");
}
```

### `switch-case`:
```java
int dia = 3;

switch (dia) {
    case 1:
        System.out.println("Domingo");
        break;
    case 2:
        System.out.println("Segunda");
        break;
    case 3:
        System.out.println("Terça");
        break;
    default:
        System.out.println("Dia inválido");
        break;
}
```

> [!TIP]
> **Switch Expression (Java 14+):** Mais limpo e não precisa de `break`:
> ```java
> String diaTexto = switch (dia) {
>     case 1 -> "Domingo";
>     case 2 -> "Segunda";
>     case 3 -> "Terça";
>     default -> "Dia inválido";
> };
> ```

---

## 6. 🔁 Estruturas de Repetição (Loops)

### `for` clássico (com contador):
```java
for (int i = 0; i < 5; i++) {
    System.out.println("Iteração: " + i);
}
```

### `while` (enquanto a condição for verdadeira):
```java
int contador = 1;
while (contador <= 3) {
    System.out.println("Contagem: " + contador);
    contador++;
}
```

### `do-while` (executa ao menos uma vez):
```java
int numero = 0;
do {
    System.out.println("Executou: " + numero);
    numero++;
} while (numero < 1);
```

### `for-each` (para percorrer arrays e coleções):
```java
String[] linguagens = {"Java", "Python", "C#"};
for (String lang : linguagens) {
    System.out.println(lang);
}
```

---

## 7. ⚙️ Métodos (Funções)

Em Java, funções pertencem a classes e são chamadas de **métodos**.

```java
public class ExemploMetodos {

    // Método sem retorno (void)
    public static void exibirMensagem(String nome) {
        System.out.println("Bem-vindo(a), " + nome);
    }

    // Método com retorno (int) e dois parâmetros
    public static int multiplicar(int a, int b) {
        return a * b;
    }

    public static void main(String[] args) {
        exibirMensagem("Dev");
        int res = multiplicar(4, 5);
        System.out.println("Resultado: " + res); // 20
    }
}
```

---

## 8. 🗃️ Arrays (Vetores de Tamanho Fixo)

Arrays em Java possuem **tamanho fixo** definido na criação e são indexados a partir de 0.

```java
// Declaração e inicialização direta
int[] numeros = {10, 20, 30, 40, 50};

// Declaração com tamanho fixo (inicializado com zeros)
int[] valores = new int[3];
valores[0] = 100;
valores[1] = 200;
valores[2] = 300;

// Propriedade length para ver o tamanho
System.out.println("Tamanho: " + numeros.length); // 5

// Acessando e modificando
System.out.println("Primeiro item: " + numeros[0]); // 10
numeros[0] = 99;
```

---

[⬅️ Voltar ao Índice Principal](../README.md)
