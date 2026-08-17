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
- **Constantes (`final`):** Equivalente ao `const` de outras linguagens (trava valor primitivo ou referência de memória).
- **Entrada e Saída:** `System.out.println()`, `System.out.printf()` e `Scanner`.
- **Operadores & Condicionais:** Aritméticos, lógicos, ternário, `if-else` e `switch`.
- **Loops & Arrays:** `for`, `while`, `for-each` e vetores indexados `tipo[]`.
- **Funções & Métodos:** Anatomia (`static`, retorno, parâmetros), *Pass-by-Value* e recursão.

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

### 02. [Sobrecarga de Métodos (Method Overloading)](topicos/02-sobrecarga-de-metodos.md)
Permite criar múltiplos métodos com o **mesmo nome**, desde que possuam **listas de parâmetros diferentes** (quantidade, tipos ou ordem).

> ⚠️ **Regra:** O **tipo de retorno não faz parte da assinatura**. Mudar apenas o tipo de retorno não é sobrecarga e gera erro de compilação.

```java
public class Notificador {
    // Sobrecarga variando quantidade e tipos de parâmetros
    public void enviar(String msg) { System.out.println("Aviso: " + msg); }
    public void enviar(String para, String msg) { System.out.println("Para " + para + ": " + msg); }
    public void enviar(String para, String msg, boolean urgente) {
        System.out.println((urgente ? "[URGENTE] " : "[INFO] ") + para + ": " + msg);
    }
}

// Chamadas válidas:
notif.enviar("Servidor reiniciado");
notif.enviar("admin@empresa.com", "Backup concluído");
notif.enviar("admin@empresa.com", "Falha crítica!", true);
```

[⬆️ Voltar ao Início](#-java-cheat-sheet)

---

### 03. [Programação Orientada a Objetos (POO / OOP)](topicos/03-programacao-orientada-a-objetos.md)
Organização do código em torno de **Classes**, **Objetos** e os **4 Pilares fundamentais**:

- **Abstração & Encapsulamento:** Atributos `private`, métodos `public` e controle de acesso.
- **Construtores & Sobrecarga:** Inicialização de instâncias e encadeamento com `this(...)`.
- **Herança (`extends`):** Reuso e especialização de código com `super(...)`.
- **Polimorfismo (`@Override`):** Execução do comportamento específico da subclasse em tempo de execução.
- **Interfaces & Classes Abstratas:** Contratos com `implements` e classes base com `abstract`.
- **Membros Estáticos (`static`):** Atributos, métodos, blocos de inicialização e classes aninhadas.
- **Imutabilidade (`final`):** Constantes, métodos não-sobrescrevíveis e classes não-estendíveis.

```java
// Superclasse Abstrata
public abstract class Animal {
    private String nome;
    public Animal(String nome) { this.nome = nome; }
    public String getNome() { return nome; }
    public abstract void emitirSom();
}

// Subclasse Concreta com Polimorfismo
public class Cachorro extends Animal {
    public Cachorro(String nome) { super(nome); }

    @Override
    public void emitirSom() {
        System.out.println(getNome() + " late: Au Au!");
    }
}

// Uso polimórfico:
Animal animal = new Cachorro("Rex");
animal.emitirSom(); // Rex late: Au Au!
```

[⬆️ Voltar ao Início](#-java-cheat-sheet)

---

### 04. [O Método toString()](topicos/04-metodo-tostring.md)
Representação textual legível do estado dos objetos herdada de `java.lang.Object`:

- **Sobrescrita (`@Override`):** Substituição da saída técnica padrão (`Classe@hash`) por atributos legíveis.
- **Chamadas Implícitas:** Execução automática em `System.out.println()`, concatenação `+` e coleções.
- **Arrays e Matrizes:** Impressão correta via `Arrays.toString()` e `Arrays.deepToString()`.
- **Java Records:** Geração nativa e automática do método `toString()`.

```java
public class Pessoa {
    private String nome;
    private int idade;

    public Pessoa(String nome, int idade) {
        this.nome = nome;
        this.idade = idade;
    }

    @Override
    public String toString() {
        return "Pessoa{nome='" + nome + "', idade=" + idade + "}";
    }
}

// Uso (chamada implícita):
Pessoa p = new Pessoa("Alice", 25);
System.out.println(p); // Saída: Pessoa{nome='Alice', idade=25}
```

[⬆️ Voltar ao Início](#-java-cheat-sheet)

---

### 05. [Polimorfismo em Tempo de Execução (Runtime Polymorphism)](topicos/05-polimorfismo-em-tempo-de-execucao.md)
Mecanismo (*Dynamic Method Dispatch*) onde a chamada de métodos sobrescritos (`@Override`) é resolvida dinamicamente pela JVM com base no **objeto real instanciado na memória Heap**:

- **Requisitos:** Herança/Interfaces, sobrescrita `@Override` e Upcasting (`SuperClasse ref = new SubClasse()`).
- **Coleções Polimórficas:** Manipulação uniforme de diferentes subtipos em arrays ou listas.
- **Restrições:** Atributos (*fields*), métodos `static`, `private` e `final` **não** sofrem polimorfismo dinâmico.
- **Casting Seguro:** Conversão de tipos com `instanceof` e *Pattern Matching*.

```java
// Interface / Superclasse
interface Veiculo { void mover(); }

class Carro implements Veiculo { 
    public void mover() { System.out.println("Carro acelerando 🚗"); } 
}
class Barco implements Veiculo { 
    public void mover() { System.out.println("Barco navegando ⛵"); } 
}

// Execução dinâmica em tempo de execução:
Veiculo[] frota = { new Carro(), new Barco() };
for (Veiculo v : frota) {
    v.mover(); // Cada objeto executa seu próprio comportamento!
}
```

[⬆️ Voltar ao Início](#-java-cheat-sheet)

---

### 06. [Agregação (Aggregation)](topicos/06-agregacao.md)
Relação de associação do tipo **"TEM-UM"** (*HAS-A*) com **acoplamento fraco**, onde os objetos associados possuem ciclos de vida independentes:

- **Ciclo de Vida Independente:** Se o objeto dono for destruído, o objeto agregado continua existindo na memória.
- **Injeção via Construtor/Setter:** Os objetos são instanciados fora e passados como parâmetros.
- **Agregação vs Composição:** Na composição a existência do filho depende do pai (`Carro`/`Motor`); na agregação os objetos são autônomos (`Biblioteca`/`Livro`).

```java
// O livro existe de forma independente
Livro livro = new Livro("Código Limpo", "Robert Martin");

// A biblioteca agrega o livro existente
Biblioteca biblio = new Biblioteca("TechLib", new Livro[]{ livro });

biblio = null; // A biblioteca é descartada
System.out.println(livro.getTitulo()); // O livro continua existindo intacto!
```

[⬆️ Voltar ao Início](#-java-cheat-sheet)

---

### 07. [Composição (Composition)](topicos/07-composicao.md)
Relação de associação do tipo **"TEM-UM"** (*HAS-A*) com **acoplamento forte**, caracterizada pela **morte compartilhada** e ciclos de vida estritamente dependentes:

- **Morte Compartilhada:** O objeto filho não tem existência independente fora da classe dona (se o pai for destruído, o filho também será).
- **Instanciação Interna:** O componente é criado diretamente dentro do construtor da classe dona com `new`.
- **Delegação de Chamadas:** A classe dona delega as operações para o componente interno encapsulado.

```java
// O Carro cria e gerencia seu próprio Motor internamente
public class Carro {
    private Motor motor;

    public Carro(String tipoMotor, int cavalos) {
        this.motor = new Motor(tipoMotor, cavalos); // Instanciação interna
    }

    public void ligar() {
        motor.ligar(); // Delegação
    }
}
```

[⬆️ Voltar ao Início](#-java-cheat-sheet)

---

### 08. [Classes Wrapper (Wrapper Classes)](topicos/08-classes-wrapper.md)
Classes utilitárias (`Integer`, `Double`, `Boolean`, `Character`) que transformam os 8 tipos primitivos em **Objetos**, permitindo seu uso com Generics, Coleções e métodos auxiliares:

- **Autoboxing & Unboxing:** Conversão automática e transparente entre primitivos e objetos wrapper.
- **Coleções & Generics:** Obrigatório para uso em estruturas como `List<Integer>` ou `Map<String, Double>`.
- **Métodos Utilitários:** Conversão de texto para número (`Integer.parseInt()`) e validação de caracteres (`Character.isLetter()`).
- **Cuidado com Cache & Nulos:** Sempre comparar wrappers com `.equals()` e evitar `NullPointerException` no unboxing de variáveis nulas.

```java
// Autoboxing e Parsing
Integer idade = 25; 
int valor = Integer.parseInt("100");

// Obrigatório em Coleções e Generics
List<Double> notas = new ArrayList<>();
notas.add(9.5); // Autoboxing automático de double para Double

// Comparação segura de conteúdo
Integer a = 200, b = 200;
System.out.println(a.equals(b)); // true
```

[⬆️ Voltar ao Início](#-java-cheat-sheet)

---

### 09. [ArrayList (Listas Dinâmicas)](topicos/09-arraylist.md)
Estrutura de dados redimensionável da *Collections Framework* que permite armazenar sequências dinâmicas de **Objetos**:

- **Array vs ArrayList:** Redimensionamento automático vs tamanho estático fixo (`tipo[]`).
- **Operações Principais:** `add()`, `get()`, `set()`, `remove()`, `size()`, `contains()` e `clear()`.
- **Iteração & Ordenação:** Percorrimento com `for-each`, lambdas e ordenação via `Collections.sort()`.
- **ArrayList 2D:** Criação de matrizes dinâmicas multidimensionais (`ArrayList<ArrayList<T>>`).

```java
import java.util.ArrayList;
import java.util.List;

// Declaração e manipulação
List<String> frutas = new ArrayList<>();
frutas.add("Maçã");
frutas.add("Banana");
frutas.set(1, "Morango"); // Substitui "Banana" por "Morango"
frutas.remove("Maçã");    // Remove por valor

System.out.println(frutas); // [Morango]
```

[⬆️ Voltar ao Início](#-java-cheat-sheet)

---





