# 🏛️ Programação Orientada a Objetos (POO / OOP) em Java

A **Programação Orientada a Objetos** é o paradigma central do Java. Ela organiza o software em torno de **Classes** (moldes) e **Objetos** (instâncias do mundo real com estado e comportamento).

---

## 🏛️ Os 4 Pilares da POO

```
┌────────────────────────────────────────────────────────┐
│                      PILARES DA POO                     │
├───────────────┬────────────────┬───────────────┬───────┤
│ 1. Abstração  │2.Encapsulamento│  3. Herança   │4.Poli.│
└───────────────┴────────────────┴───────────────┴───────┘
```

1. **Abstração:** Isolar as características e comportamentos essenciais de um objeto, ocultando detalhes complexos.
2. **Encapsulamento:** Proteger o estado interno do objeto contra acesso direto indevido, expondo métodos controlados (`getters`/`setters`).
3. **Herança:** Permitir que uma classe filha herde atributos e métodos de uma classe mãe (`extends`), promovendo o reuso de código.
4. **Polimorfismo:** Capacidade de um objeto assumir muitas formas (um método executar comportamentos diferentes dependendo do objeto real que o executa via `@Override`).

---

## 1. 📦 Classes, Objetos e Construtores

- **Classe:** O modelo/planta (ex: `Carro`).
- **Objeto:** A instância concreta em memória (ex: `meuCarro = new Carro(...)`).
- **Construtor:** Método especial com o mesmo nome da classe, executado na instanciação com `new`.

```java
public class Carro {
    // Atributos (Estado)
    private String marca;
    private String modelo;
    private int velocidade;

    // Construtor
    public Carro(String marca, String modelo) {
        this.marca = marca;
        this.modelo = modelo;
        this.velocidade = 0;
    }

    // Métodos (Comportamento)
    public void acelerar(int incremento) {
        this.velocidade += incremento;
        System.out.println(modelo + " acelerou para " + this.velocidade + " km/h");
    }
}
```

### 🏗️ Sobrecarga de Construtores (*Constructor Overloading*)

Permite definir múltiplos construtores em uma mesma classe com **listas de parâmetros diferentes**, dando flexibilidade para instanciar objetos com valores parciais, padrão ou completos.

Podemos utilizar `this(...)` para encadear construtores (*constructor chaining*) e evitar duplicação de código.

> [!IMPORTANT]
> A chamada `this(...)` para outro construtor deve ser **obrigatoriamente a primeira linha** dentro do corpo do construtor.

```java
public class Pizza {
    private String massa;
    private String molho;
    private String queijo;
    private String cobertura;

    // Construtor 1: Completo (4 ingredientes)
    public Pizza(String massa, String molho, String queijo, String cobertura) {
        this.massa = massa;
        this.molho = molho;
        this.queijo = queijo;
        this.cobertura = cobertura;
    }

    // Construtor 2: Sem cobertura (chama o construtor 1 com valor padrão)
    public Pizza(String massa, String molho, String queijo) {
        this(massa, molho, queijo, "Nenhuma");
    }

    // Construtor 3: Simples (apenas massa e molho)
    public Pizza(String massa, String molho) {
        this(massa, molho, "Sem queijo", "Nenhuma");
    }

    // Construtor 4: Padrão / Sem argumentos
    public Pizza() {
        this("Tradicional", "Tomate", "Mussarela", "Manjericão");
    }
}

// Exemplos de instanciação:
Pizza p1 = new Pizza("Fina", "Tomate", "Mussarela", "Calabresa");
Pizza p2 = new Pizza("Grossa", "Tomate", "Mussarela");
Pizza p3 = new Pizza(); // Padrão
```

---

## 2. 🛡️ Modificadores de Acesso e Encapsulamento

O encapsulamento restringe o acesso direto aos campos usando atributos `private` e métodos `public` para leitura (`get`) e modificação validada (`set`).

### Tabela de Visibilidade dos Modificadores:

| Modificador | Mesma Classe | Mesmo Pacote | Subclasses (Herança) | Todo o Projeto |
| :--- | :---: | :---: | :---: | :---: |
| `public` | ✅ | ✅ | ✅ | ✅ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| *(padrão / package-private)* | ✅ | ✅ | ❌ | ❌ |
| `private` | ✅ | ❌ | ❌ | ❌ |

### Exemplo de Encapsulamento:

```java
public class ContaBancaria {
    private double saldo; // Protegido contra alteração direta externa

    public ContaBancaria(double saldoInicial) {
        this.saldo = Math.max(0, saldoInicial);
    }

    public double getSaldo() {
        return this.saldo;
    }

    public void depositar(double valor) {
        if (valor > 0) {
            this.saldo += valor;
        }
    }
}
```

---

## 3. 🧬 Herança (`extends` e `super`)

A herança cria uma relação **"é um"** (*is-a*). Em Java, uma classe só pode herdar diretamente de **uma única superclasse** (herança simples).

- `super(...)`: Chama o construtor da superclasse.
- `super.metodo()`: Invoca o método da superclasse.

```java
// Superclasse (Mãe)
public class Animal {
    protected String nome;

    public Animal(String nome) {
        this.nome = nome;
    }

    public void emitirSom() {
        System.out.println(nome + " faz algum som...");
    }
}

// Subclasse (Filha)
public class Cachorro extends Animal {

    public Cachorro(String nome) {
        super(nome); // Chama o construtor de Animal
    }

    @Override
    public void emitirSom() {
        System.out.println(nome + " late: Au Au!");
    }
}
```

---

## 4. 🎭 Polimorfismo Dinâmico (Sobrescrita / `@Override`)

O polimorfismo permite tratar um objeto especializado como sendo do seu tipo mais genérico:

```java
public class TestePolimorfismo {
    public static void main(String[] args) {
        // Referência do tipo Animal apontando para objeto Cachorro
        Animal meuAnimal = new Cachorro("Rex");
        
        // Executa a versão sobrescrita em Cachorro em tempo de execução!
        meuAnimal.emitirSom(); // Saída: Rex late: Au Au!
    }
}
```

---

## 5. 🧩 Classes Abstratas vs Interfaces

| Característica | Classe Abstrata (`abstract class`) | Interface (`interface`) |
| :--- | :--- | :--- |
| **Palavra-chave** | `abstract class` (usa `extends`) | `interface` (usa `implements`) |
| **Herança Múltipla** | ❌ Não (Apenas herança simples) | ✅ Sim (Pode implementar várias) |
| **Atributos** | Pode ter variáveis de instância normais | Apenas constantes (`public static final`) |
| **Construtores** | ✅ Possui construtores | ❌ Não possui construtores |
| **Uso Ideal** | Relação forte de identidade ("é um") | Contrato de comportamento ("pode fazer") |

### Exemplo de Interface e Classe Abstrata:

```java
// Interface (Contrato)
public interface Autenticavel {
    boolean autenticar(String senha); // Método abstrato por padrão
}

// Classe Abstrata
public abstract class Funcionario {
    protected String nome;
    protected double salario;

    public Funcionario(String nome, double salario) {
        this.nome = nome;
        this.salario = salario;
    }

    public abstract double calcularBonificacao(); // Obrigatório implementar na filha
}

// Classe Concreta herdando e implementando interface
public class Gerente extends Funcionario implements Autenticavel {
    public Gerente(String nome, double salario) {
        super(nome, salario);
    }

    @Override
    public double calcularBonificacao() {
        return this.salario * 0.20;
    }

    @Override
    public boolean autenticar(String senha) {
        return "admin123".equals(senha);
    }
}
```

---

## 6. 🔑 Palavras-Chave Especiais: `static` e `final`

### `static` (Membro de Classe):
Pertence à **classe**, não às instâncias individuais. Pode ser chamado sem instanciar com `new`.
```java
public class Configuracao {
    public static int contadorInstancias = 0;

    public Configuracao() {
        contadorInstancias++;
    }

    public static void exibirInfo() {
        System.out.println("Instâncias criadas: " + contadorInstancias);
    }
}
// Uso: Configuracao.exibirInfo();
```

### `final`:
- **Variável `final`:** Constante (não pode ser reatribuída).
- **Método `final`:** Não pode ser sobrescrito (`@Override`) por subclasses.
- **Classe `final`:** Não pode ser estendida (`extends`), como a classe `String` e `Math`.

---

[⬅️ Voltar ao Índice Principal](../README.md)
