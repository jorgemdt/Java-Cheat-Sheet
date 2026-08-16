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

## 6. ⚡ O Modificador `static` (Membros de Classe)

A palavra-chave `static` indica que um membro (atributo, método, bloco ou classe aninhada) pertence à **classe como um todo**, e não a uma **instância individual (objeto)**.

```
┌────────────────────────────────────────────────────────┐
│               MEMÓRIA / ESCOPO NO JAVA                │
├──────────────────────────┬─────────────────────────────┤
│   Área da Classe (Meta)  │        Heap (Objetos)       │
│  ┌────────────────────┐  │  ┌───────────────────────┐  │
│  │ static int total;  │  │  │ Objeto 1 { id: 101 }  │  │
│  │ static void log(); │  │  │ Objeto 2 { id: 102 }  │  │
│  │ (Único na memória) │  │  │ Objeto 3 { id: 103 }  │  │
│  └────────────────────┘  │  └───────────────────────┘  │
└──────────────────────────┴─────────────────────────────┘
```

---

### 1. Atributos Estáticos (*Static Variables / Class Variables*)
- Compartilhados por **todos os objetos** daquela classe.
- Se uma instância altera um atributo estático, todas as outras instâncias enxergam a alteração imediatamente.
- Alocados na memória apenas uma vez, quando a classe é carregada pela JVM.

```java
public class Conta {
    private String titular;
    private double saldo;
    
    // Atributo estático: compartilhado por todas as contas
    public static int totalDeContas = 0;

    public Conta(String titular, double saldo) {
        this.titular = titular;
        this.saldo = saldo;
        Conta.totalDeContas++; // Incrementa a contagem global
    }
}

// Uso:
Conta c1 = new Conta("Alice", 1000.0);
Conta c2 = new Conta("Bob", 2000.0);

System.out.println(Conta.totalDeContas); // Saída: 2 (acesso recomendado via NomeDaClasse)
```

#### Constantes Globais (`public static final`):
Quando combinado com `final`, cria uma constante imutável e acessível globalmente:
```java
public class Constantes {
    public static final double PI = 3.141592653589793;
    public static final String SISTEMA_VERSAO = "v2.1.0";
}
```

---

### 2. Métodos Estáticos (*Static Methods*)
- Podem ser executados **diretamente pela classe**, sem necessidade de instanciar um objeto (`new`).
- Comumente usados para:
  - **Métodos utilitários:** ex. `Math.sqrt()`, `Collections.sort()`, `Integer.parseInt()`.
  - **Fábricas estáticas (*Factory Methods*):** ex. `List.of()`, `LocalDate.now()`, `Optional.of()`.
  - **Ponto de entrada da aplicação:** `public static void main(String[] args)`.

```java
public class CalculadoraUtil {
    
    // Método utilitário estático
    public static int somar(int a, int b) {
        return a + b;
    }
    
    public static boolean ehPar(int numero) {
        return numero % 2 == 0;
    }
}

// Chamada direta pela classe:
int resultado = CalculadoraUtil.somar(15, 30);
boolean par = CalculadoraUtil.ehPar(10);
```

> [!WARNING]
> **Regras de Ouro dos Métodos Estáticos:**
> 1. **NÃO podem usar `this` ou `super`**, pois não há instância associada na chamada.
> 2. **NÃO podem acessar atributos ou métodos de instância diretamente**. Devem instanciar um objeto primeiro se precisarem manipulá-los.
> 3. **NÃO sofrem polimorfismo dinâmico (`@Override`)**: se uma subclasse declarar um método estático com a mesma assinatura, ocorre **ocultação de método (*Method Hiding*)**, e não sobrescrita.

```java
public class ExemploContexto {
    private int valorInstancia = 10;
    private static int valorEstatico = 20;

    public static void metodoEstatico() {
        // System.out.println(valorInstancia); // ❌ ERRO DE COMPILAÇÃO!
        // System.out.println(this.valorInstancia); // ❌ ERRO: 'this' não pode ser referenciado em contexto estático
        
        System.out.println(valorEstatico); // ✅ Válido!
    }
}
```

---

### 3. Blocos de Inicialização Estáticos (*Static Initializer Blocks*)
- O bloco `static { ... }` é executado **uma única vez**, no momento exato em que a classe é carregada na memória pela JVM.
- Executado **antes de qualquer construtor** e antes de qualquer instância ser criada.
- Ideal para inicialização complexa de variáveis estáticas, carregamento de configurações, drivers ou leitura de arquivos de propriedades.

```java
import java.util.HashMap;
import java.util.Map;

public class ConfiguracaoBanco {
    public static final Map<String, String> CONFIGS = new HashMap<>();

    // Bloco de inicialização estático
    static {
        System.out.println("Carregando configurações na memória...");
        CONFIGS.put("db.host", "localhost");
        CONFIGS.put("db.port", "5432");
        CONFIGS.put("db.user", "admin");
    }
}
```

---

### 4. Importação Estática (*Static Import*)
Permite acessar membros estáticos (campos e métodos) de outra classe diretamente pelo nome, sem precisar prefixar com o nome da classe:

```java
// Sem static import:
double area = Math.PI * Math.pow(5.0, 2);
System.out.println("Área: " + area);

// Com static import:
import static java.lang.Math.PI;
import static java.lang.Math.pow;
import static java.lang.System.out;

double area = PI * pow(5.0, 2);
out.println("Área: " + area);
```

> [!TIP]
> Use static import com bom senso para não poluir o escopo nem prejudicar a legibilidade do código. É muito comum e recomendado em bibliotecas de testes (como `Assertions.*` do JUnit e `Mockito.*`).

---

### 5. Classes Aninhadas Estáticas (*Static Nested Classes*)
Uma classe declarada dentro de outra com o modificador `static`:
- **Não precisa de uma instância da classe externa** para existir.
- Não mantém referência oculta para a instância da classe externa (mais leve e previne vazamentos de memória).

```java
public class BancoDeDados {
    private String nome = "Producao";

    // Classe aninhada estática
    public static class ConexaoConfig {
        private String timeout;

        public ConexaoConfig(String timeout) {
            this.timeout = timeout;
        }

        public void conectar() {
            System.out.println("Conectando com timeout: " + timeout);
        }
    }
}

// Instanciação direta sem precisar de new BancoDeDados():
BancoDeDados.ConexaoConfig config = new BancoDeDados.ConexaoConfig("30s");
config.conectar();
```

---

### 📊 Comparativo: Instância vs `static`

| Característica | Membro de Instância (Normal) | Membro Estático (`static`) |
| :--- | :--- | :--- |
| **Pertence a** | Cada objeto individual (`new`) | À classe globalmente |
| **Alocação de Memória** | Uma cópia por objeto no *Heap* | Apenas uma cópia no *Metaspace / Class Area* |
| **Acesso Recomendado** | Via referência do objeto (`objeto.metodo()`) | Via nome da classe (`Classe.metodo()`) |
| **Uso de `this` e `super`** | ✅ Permitido | ❌ Proibido |
| **Ciclo de Vida** | Criado com `new`, destruído pelo Garbage Collector | Carregado com a classe, persiste até o fim da JVM |

---

## 7. 🔒 O Modificador `final` (Imutabilidade e Restrições)

A palavra-chave `final` é usada para aplicar **imutabilidade e restrições** em 3 níveis:

### 1. Variáveis e Atributos `final`:
- O valor não pode ser alterado após a inicialização (constante).
- Se for uma referência de objeto, o ponteiro não pode mudar (mas o estado interno do objeto referenciado ainda pode ser mutável).

```java
final double TAXA_JUROS = 0.05;
// TAXA_JUROS = 0.08; // ❌ ERRO DE COMPILAÇÃO: não pode ser reatribuído

final List<String> nomes = new ArrayList<>();
nomes.add("Carlos"); // ✅ Permitido (modifica o conteúdo do objeto)
// nomes = new ArrayList<>(); // ❌ ERRO: não pode apontar para outro objeto
```

### 2. Métodos `final`:
- O método **não pode ser sobrescrito** (`@Override`) por nenhuma subclasse. Garante que o comportamento original seja preservado com segurança.

```java
public class ContaSegura {
    public final void emitirComprovante() {
        System.out.println("Comprovante autenticado e imutável.");
    }
}
```

### 3. Classes `final`:
- A classe **não pode ser herdada / estendida** (`extends`).
- Exemplos nativos do Java: `String`, `Integer`, `Double`, `Math`, `System`.

```java
public final class TokenAutenticacao {
    // Nenhuma classe pode fazer: class HackerToken extends TokenAutenticacao
}
```

---

[⬅️ Voltar ao Índice Principal](../README.md)
