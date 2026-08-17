# 🎭 Polimorfismo em Tempo de Execução (Runtime Polymorphism) em Java

O **Polimorfismo em Tempo de Execução** (*Runtime Polymorphism* ou *Dynamic Method Dispatch*) é o mecanismo pelo qual uma chamada a um método sobrescrito (`@Override`) é resolvida **dinamicamente durante a execução do programa (runtime)**, e não em tempo de compilação.

A decisão de qual versão do método executar depende exclusivamente do **tipo do objeto real instanciado na memória Heap** (à direita do `new`), e não do tipo da variável de referência (à esquerda).

---

## ⚖️ Polimorfismo Estático vs Dinâmico

| Característica | Polimorfismo Estático (Tempo de Compilação) | Polimorfismo Dinâmico (Tempo de Execução) |
| :--- | :--- | :--- |
| **Mecanismo** | **Sobrecarga de Métodos** (*Method Overloading*) | **Sobrescrita de Métodos** (*Method Overriding*) |
| **Quando é resolvido?** | Durante a compilação (*Compile-time*) | Durante a execução (*Runtime*) |
| **Como é diferenciado?** | Pela assinatura (quantidade/tipos de parâmetros) | Pelo objeto real instanciado na memória Heap |
| **Requer Herança?** | ❌ Não | ✅ Sim (`extends` ou `implements`) |
| **Desempenho** | Ligeiramente mais rápido (ligação direta) | Despacho dinâmico via tabela virtual (*vtable*) |

---

## ⚙️ Os 3 Requisitos do Runtime Polymorphism

Para que o polimorfismo dinâmico aconteça, são necessários:
1. **Relação de Herança ou Interface:** Subclasses estendendo uma superclasse (`extends`) ou implementando uma interface (`implements`).
2. **Sobrescrita de Método:** A subclasse deve sobrescrever o método com a mesma assinatura usando `@Override`.
3. **Upcasting na Referência:** Uma variável do tipo da **superclasse/interface** apontando para o objeto da **subclasse**.

$$\text{SuperClasse } \text{ref} = \mathbf{new \ SubClasse()};$$

---

## 💻 Exemplos Práticos

### 1. Decisão Dinâmica com Escolha em Runtime (Estilo Bro Code)

Neste exemplo clássico, o tipo do objeto só é decidido quando o usuário digita no terminal durante a execução:

```java
import java.util.Scanner;

// Superclasse genérica
class Animal {
    public void emitirSom() {
        System.out.println("O animal faz algum som...");
    }
}

// Subclasses especializadas
class Cachorro extends Animal {
    @Override
    public void emitirSom() {
        System.out.println("O cachorro late: Au Au! 🐶");
    }
}

class Gato extends Animal {
    @Override
    public void emitirSom() {
        System.out.println("O gato mia: Miau! 🐱");
    }
}

public class ExemploRuntimeBasico {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Animal animalEscolhido; // Referência polimórfica (tipo Animal)

        System.out.println("Escolha um animal: (1) Cachorro ou (2) Gato");
        int opcao = scanner.nextInt();

        if (opcao == 1) {
            animalEscolhido = new Cachorro();
        } else if (opcao == 2) {
            animalEscolhido = new Gato();
        } else {
            animalEscolhido = new Animal();
        }

        // A chamada é IDÊNTICA, mas o comportamento depende do objeto real na memória!
        animalEscolhido.emitirSom();

        scanner.close();
    }
}
```

---

### 2. Coleções e Arrays Polimórficos

Permite manipular dezenas de objetos diferentes através de uma interface ou superclasse comum, eliminando blocos repetitivos de `if` ou `switch`:

```java
// Interface comum
interface Veiculo {
    void mover();
    double calcularConsumo(double distanciaKm);
}

class Carro implements Veiculo {
    @Override
    public void mover() {
        System.out.println("Carro acelerando no asfalto: Vrum Vrum! 🚗");
    }

    @Override
    public double calcularConsumo(double distanciaKm) {
        return distanciaKm / 12.0; // 12 km/l
    }
}

class Bicicleta implements Veiculo {
    @Override
    public void mover() {
        System.out.println("Pedalando a bicicleta na ciclovia: Trim Trim! 🚲");
    }

    @Override
    public double calcularConsumo(double distanciaKm) {
        return 0.0; // Não consome combustível
    }
}

class Barco implements Veiculo {
    @Override
    public void mover() {
        System.out.println("Barco navegando nas águas... ⛵");
    }

    @Override
    public double calcularConsumo(double distanciaKm) {
        return distanciaKm / 4.0; // 4 km/l
    }
}

public class FrotaPolimorfica {
    public static void main(String[] args) {
        // Array polimórfico de Veículos
        Veiculo[] frota = {
            new Carro(),
            new Bicicleta(),
            new Barco()
        };

        for (Veiculo v : frota) {
            v.mover(); // Cada um executa a sua versão em tempo de execução!
            System.out.println("Combustível para 100km: " + v.calcularConsumo(100) + "L\n");
        }
    }
}
```

---

## ⚠️ O que NÃO participa do Runtime Polymorphism?

> [!CAUTION]
> Nem tudo em Java é resolvido dinamicamente em tempo de execução. Fique atento a estas exceções:

### 1. Atributos / Variáveis (*Data Members*) NÃO são polimórficos!
O acesso a atributos é resolvido em **tempo de compilação** baseado no **tipo da referência**:

```java
class Pai {
    int valor = 10;
}

class Filho extends Pai {
    int valor = 20; // Oculta o atributo da classe Pai (Field Shadowing)
}

public class TesteAtributo {
    public static void main(String[] args) {
        Pai obj = new Filho();
        System.out.println(obj.valor); // Saída: 10 (tipo da referência 'Pai', NÃO 20!)
    }
}
```

### 2. Métodos `static` NÃO sofrem sobrescrita (*Method Hiding*)
Métodos estáticos estão ligados à classe, não à instância:

```java
class Base {
    public static void metodo() { System.out.println("Base"); }
}

class Derivada extends Base {
    public static void metodo() { System.out.println("Derivada"); }
}

Base b = new Derivada();
b.metodo(); // Saída: Base (porque 'b' é do tipo Base)
```

### 3. Métodos `private` e `final`
- Métodos `private` não são visíveis para subclasses e não podem ser sobrescritos.
- Métodos marcados com `final` são impedidos de serem sobrescritos pelo compilador.

---

## 🔄 Casting de Objetos e o Operador `instanceof`

### Upcasting (Automático e Seguro)
Converter uma referência de subclasse para superclasse:
```java
Animal animal = new Cachorro(); // Upcasting implícito
```

### Downcasting (Explícito e Potencialmente Perigoso)
Converter uma referência de superclasse de volta para a subclasse concreta. Se o objeto real não for do tipo esperado, uma `ClassCastException` será lançada em tempo de execução:

```java
// ❌ Sem verificação: Risco de ClassCastException!
Animal a = new Gato();
// Cachorro c = (Cachorro) a; // LANÇA ClassCastException!

// ✅ Forma Segura com Pattern Matching for instanceof (Java 16+):
if (a instanceof Cachorro c) {
    c.emitirSom();
} else {
    System.out.println("O animal não é um cachorro.");
}
```

---

## 📋 Resumo Rápido

| Conceito | Regra |
| :--- | :--- |
| **Resolução** | Em tempo de execução (Runtime) pela JVM via Dynamic Method Dispatch |
| **Critério** | Determinado pelo objeto real (`new SubClasse()`) na Heap |
| **Métodos** | Sobrescritos com `@Override` |
| **Variáveis/Campos** | NÃO possuem polimorfismo dinâmico (ligação estática) |
| **Métodos `static` / `final` / `private`** | NÃO participam do polimorfismo dinâmico |
| **Principal Benefício** | Extensibilidade, código desacoplado e aberto para novas subclasses sem alterar a lógica consumidora (Princípio Open/Closed do SOLID) |

---

[⬅️ Voltar ao Índice Principal](../README.md)
