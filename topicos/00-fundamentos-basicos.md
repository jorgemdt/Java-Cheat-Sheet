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

## 7. ⚙️ Funções e Métodos

Em Java, **não existem funções soltas/globais** fora de classes. Todas as funções são membros de uma classe e chamadas de **métodos**.

---

### 7.1 Anatomia de um Método

```java
// [modificador] [static/opcional] [tipo_retorno] nomeDoMetodo([parâmetros]) { ... }

public static int somarValores(int a, int b) {
    int resultado = a + b;
    return resultado; // Retorna o valor compatível com o tipo definido
}
```

- **Modificador de Acesso (`public`, `private`):** Define quem pode chamar o método.
- **`static`:** Permite chamar o método diretamente pela classe (sem precisar de `new`).
- **Tipo de Retorno:** O tipo do dado que o método devolve (`int`, `String`, `boolean`, etc.) ou `void` caso não retorne nada.
- **Nome do Método:** Por convenção, utiliza-se **`camelCase`** iniciando com verbo (ex: `calcularMedia`, `obterNome`).
- **Parâmetros:** Variáveis declaradas na assinatura que recebem os dados de entrada.

---

### 7.2 Métodos `void` vs Métodos com Retorno

#### Método sem retorno (`void`) com Retorno Antecipado (*Early Return*):
```java
public static void verificarIdade(int idade) {
    // Cláusula de guarda (Early Return)
    if (idade < 0) {
        System.out.println("Idade inválida!");
        return; // Interrompe a execução do método imediatamente
    }

    if (idade >= 18) {
        System.out.println("Acesso liberado.");
    } else {
        System.out.println("Acesso restrito.");
    }
}
```

#### Método com Retorno:
Todo caminho de execução deve, obrigatoriamente, alcançar uma instrução `return` com um valor compatível:
```java
public static boolean ehPar(int numero) {
    return numero % 2 == 0;
}
```

---

### 7.3 Passagem por Valor (*Pass-by-Value*)

> [!IMPORTANT]
> Em Java, **todos os argumentos são passados estritamente por valor** (uma cópia é entregue ao método).

- **Com tipos primitivos (`int`, `double`, etc.):** Uma cópia do valor literal é passada. Alterar a variável dentro do método **não** altera a variável original de fora.
- **Com objetos e arrays:** Uma cópia da **referência** de memória é passada. Você pode alterar os elementos internos do objeto/array, mas reatribuir a variável do parâmetro não afeta a variável externa.

```java
public class PassagemPorValor {

    public static void alterarPrimitivo(int x) {
        x = 99; // Altera apenas a cópia local
    }

    public static void alterarArray(int[] arr) {
        arr[0] = 999; // Altera o elemento do array referenciado!
    }

    public static void main(String[] args) {
        int numero = 10;
        alterarPrimitivo(numero);
        System.out.println("Número continua: " + numero); // 10

        int[] lista = {1, 2, 3};
        alterarArray(lista);
        System.out.println("Array modificado: " + lista[0]); // 999
    }
}
```

---

### 7.4 Escopo de Variáveis (*Variable Scope*)

- **Variáveis Locais:** Declaradas dentro do método ou bloco `{ }`. Elas nascem no início do bloco e são destruídas ao fim dele.
- Não podem ser acessadas fora do seu respectivo bloco ou método.

```java
public static void exemploEscopo() {
    int total = 100; // Visível em todo o método

    if (total > 50) {
        int bonus = 20; // Visível apenas dentro deste bloco if
        System.out.println(total + bonus);
    }

    // System.out.println(bonus); // ERRO: 'bonus' não existe fora do if!
}
```

---

### 7.5 Recursividade (*Recursion*)

Um método recursivo é aquele que **chama a si mesmo** para resolver um problema menor até atingir uma **condição de parada (*Base Case*)**.

```java
public class ExemploRecursao {

    // Fatorial: n! = n * (n - 1)!
    public static int fatorial(int n) {
        // Caso base (condição de parada)
        if (n <= 1) {
            return 1;
        }
        // Chamada recursiva
        return n * fatorial(n - 1);
    }

    public static void main(String[] args) {
        System.out.println("Fatorial de 5: " + fatorial(5)); // 120
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
