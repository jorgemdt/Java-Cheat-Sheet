# 🧩 Composição (Composition) em Java

A **Composição** (*Composition*) é a forma mais forte de **Associação** em Programação Orientada a Objetos. Trata-se de uma relação do tipo **"TEM-UM"** (*HAS-A*), caracterizada por um **acoplamento forte** e **ciclos de vida estritamente vinculados**.

Na composição, o objeto componente (filho) **não pode existir de forma independente** fora do objeto dono (pai). Se o objeto pai for destruído ou coletado pelo Garbage Collector, o objeto filho **também será destruído**.

---

## 🧭 Agregação vs Composição

```
Associação ("TEM-UM")
 ├── Agregação (Vínculo Fraco - Ciclos de vida independentes)  → Ex: Biblioteca e Livro
 └── Composição (Vínculo Forte - Morte compartilhada)          → Ex: Carro e Motor
```

| Critério | Agregação | Composição |
| :--- | :--- | :--- |
| **Vínculo** | Fraco (*Loose Coupling*) | Forte (*Tight Containment*) |
| **Ciclo de Vida** | Independente | **Compartilhado / Vinculado** |
| **Instanciação** | O objeto filho é criado **fora** e injetado | O objeto filho é instanciado **dentro** da classe pai |
| **Se o pai for destruído?** | O filho **continua existindo** | O filho **é destruído junto** |
| **Exemplo Real** | `Time` e `Jogador`, `Biblioteca` e `Livro` | `Carro` e `Motor`, `Computador` e `Processador` |

---

## ⚙️ Como Implementar Composição em Java

Para garantir a composição:
1. A classe dona instancia o objeto filho **diretamente em seu próprio construtor** usando `new`.
2. A classe dona controla totalmente a criação e destruição do objeto interno.
3. A classe dona **delega** chamadas de métodos para o componente interno (*Design Pattern: Delegation*).

---

## 💻 Exemplos Práticos

### 1. Exemplo Clássico: Carro e Motor (Engine)

O motor nasce e morre junto com o carro:

```java
// Componente interno (não faz sentido existir solto no sistema)
class Motor {
    private String tipo;
    private int cavalos;

    public Motor(String tipo, int cavalos) {
        this.tipo = tipo;
        this.cavalos = cavalos;
    }

    public void ligar() {
        System.out.println("Motor " + tipo + " de " + cavalos + "cv deu partida: Vrummm! ⚙️");
    }

    public void desligar() {
        System.out.println("Motor desligado.");
    }
}

// Classe dona (Container)
class Carro {
    private String modelo;
    private Motor motor; // Composição: o Carro É DONO do Motor

    public Carro(String modelo, String tipoMotor, int cavalos) {
        this.modelo = modelo;
        // O motor é criado DIRETAMENTE aqui dentro no momento em que o carro nasce
        this.motor = new Motor(tipoMotor, cavalos);
    }

    public void ligarCarro() {
        System.out.println("Ligando " + modelo + "...");
        motor.ligar(); // Delegação de responsabilidade
    }

    public void desligarCarro() {
        System.out.println("Desligando " + modelo + "...");
        motor.desligar();
    }
}

public class ExemploComposicao {
    public static void main(String[] args) {
        Carro meuCarro = new Carro("Mustang GT", "V8 5.0", 480);
        meuCarro.ligarCarro();
        meuCarro.desligarCarro();

        // Se o carro for destruído, o motor que estava dentro dele também deixa de existir
        meuCarro = null; 
    }
}
```

---

### 2. Exemplo: Computador com Processador e Memória RAM

Demonstrando múltiplos componentes em uma composição:

```java
class Processador {
    private String modelo;
    private double frequenciaGhz;

    public Processador(String modelo, double frequenciaGhz) {
        this.modelo = modelo;
        this.frequenciaGhz = frequenciaGhz;
    }

    public void processar() {
        System.out.println("Processador " + modelo + " (" + frequenciaGhz + " GHz) processando dados...");
    }
}

class MemoriaRAM {
    private int capacidadeGb;

    public MemoriaRAM(int capacidadeGb) {
        this.capacidadeGb = capacidadeGb;
    }

    public void carregarDados() {
        System.out.println("Carregando módulos na memória de " + capacidadeGb + " GB...");
    }
}

class Computador {
    private String marca;
    private Processador cpu; // Composição
    private MemoriaRAM ram;   // Composição

    public Computador(String marca, String modeloCpu, double freqCpu, int ramGb) {
        this.marca = marca;
        this.cpu = new Processador(modeloCpu, freqCpu); // Instanciação interna
        this.ram = new MemoriaRAM(ramGb);               // Instanciação interna
    }

    public void inicializar() {
        System.out.println("Iniciando Computador " + marca + "...");
        ram.carregarDados();
        cpu.processar();
        System.out.println("Sistema pronto para uso! 💻");
    }
}

public class ExemploComputador {
    public static void main(String[] args) {
        Computador pc = new Computador("Dell XPS", "Intel Core i7", 4.8, 32);
        pc.inicializar();
    }
}
```

---

## 🎯 Por que a Composição é tão recomendada? (*Favor Composition over Inheritance*)

A máxima de orientação a objetos recomenda compor objetos em vez de estender classes (*herança*) pelos seguintes motivos:

1. **Evita o problema da Classe Base Frágil (*Fragile Base Class*):** Na herança, alterar um método na classe mãe pode quebrar inadvertidamente todas as subclasses. Na composição, a interface interna fica isolada.
2. **Encapsulamento Total:** Os detalhes internos do componente (`Motor`, `CPU`) ficam ocultos do mundo externo.
3. **Delegação Limpa:** A classe dona expõe apenas os métodos convenientes, sem ser obrigada a herdar dezenas de métodos desnecessários.

---

## 📋 Resumo Rápido

| Característica | Detalhe |
| :--- | :--- |
| **Tipo de Relação** | `HAS-A` ("TEM-UM") Forte |
| **Ciclo de Vida** | Estritamente dependente (morte compartilhada) |
| **Criação do Objeto** | Instanciado internamente com `new` pela classe dona |
| **Acesso Externo** | O componente interno não é exposto desprotegido |
| **Padrão de Execução** | Delegação de chamadas (`carro.ligar()` $\rightarrow$ `motor.ligar()`) |

---

[⬅️ Voltar ao Índice Principal](../README.md)
