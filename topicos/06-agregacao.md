# 🤝 Agregação (Aggregation) em Java

A **Agregação** (*Aggregation*) é uma forma especializada de **Associação** entre duas classes que representa uma relação do tipo **"TEM-UM"** (*HAS-A*), caracterizada por um **acoplamento fraco** (*weak association*).

Na agregação, ambos os objetos possuem **ciclos de vida independentes**. Isso significa que se o objeto principal (recipiente) for destruído ou descartado da memória, o objeto contido **continua existindo de forma independente**.

---

## 🧭 Relações em POO: Associação vs Agregação vs Composição

```
Associação (Relação genérica "usa um")
    │
    └── Agregação ("TEM-UM" Fraco - Ciclos de vida independentes)
            │
            └── Composição ("TEM-UM" Forte - Ciclos de vida vinculados / morte compartilhada)
```

| Tipo de Relação | Vínculo | Dependência do Ciclo de Vida | Exemplo do Mundo Real |
| :--- | :---: | :--- | :--- |
| **Herança** (*IS-A*) | Fortíssimo | A subclasse é uma especialização da superclasse | `Cachorro` **é um** `Animal` |
| **Composição** (*HAS-A*) | Forte | O objeto filho **não sobrevive** sem o objeto pai | `Carro` e `Motor` / `Humano` e `Coração` |
| **Agregação** (*HAS-A*) | **Fraco** | O objeto filho **sobrevive** independentemente | `Biblioteca` e `Livro` / `Time` e `Jogador` |

---

## ⚙️ Como Implementar Agregação em Java

Na agregação:
1. O objeto associado é criado **fora** da classe recipiente.
2. Ele é passado para a classe via **parâmetro de Construtor** ou método **Setter** (*Injeção de Dependência*).
3. A classe recipiente **não** instancia o objeto diretamente com `new` internamente.

---

## 💻 Exemplos Práticos

### 1. Exemplo Clássico: Livro e Biblioteca

Se a biblioteca for destruída (referência anulada / coletada pelo Garbage Collector), os livros continuam intactos na memória:

```java
// Objeto que pode existir sozinho
class Livro {
    private String titulo;
    private String autor;

    public Livro(String titulo, String autor) {
        this.titulo = titulo;
        this.autor = autor;
    }

    public String getTitulo() { return titulo; }
    public String getAutor() { return autor; }
}

// Classe que agrega Livros
class Biblioteca {
    private String nome;
    private Livro[] livros; // Agregação (array de referências de Livros)

    // Os livros são passados prontos de fora (Injeção via Construtor)
    public Biblioteca(String nome, Livro[] livros) {
        this.nome = nome;
        this.livros = livros;
    }

    public void listarAcervo() {
        System.out.println("=== " + nome + " ===");
        for (Livro livro : livros) {
            System.out.println("📖 " + livro.getTitulo() + " (Autor: " + livro.getAutor() + ")");
        }
    }
}

public class ExemploAgregacao {
    public static void main(String[] args) {
        // 1. Criamos os livros de forma independente
        Livro l1 = new Livro("O Senhor dos Anéis", "J.R.R. Tolkien");
        Livro l2 = new Livro("Código Limpo", "Robert C. Martin");

        Livro[] acervo = {l1, l2};

        // 2. Agregamos os livros à biblioteca
        Biblioteca biblioteca = new Biblioteca("Biblioteca Municipal", acervo);
        biblioteca.listarAcervo();

        // 3. Se destruirmos a biblioteca...
        biblioteca = null; 

        // 4. Os livros continuam perfeitamente acessíveis e válidos!
        System.out.println("\nLivro ainda existe: " + l1.getTitulo()); // O Senhor dos Anéis
    }
}
```

---

### 2. Exemplo com Coleções (`List`): Time e Jogador

```java
import java.util.ArrayList;
import java.util.List;

class Jogador {
    private String nome;
    private String posicao;

    public Jogador(String nome, String posicao) {
        this.nome = nome;
        this.posicao = posicao;
    }

    public String getNome() { return nome; }
    public String getPosicao() { return posicao; }
}

class Time {
    private String nomeTime;
    private List<Jogador> jogadores; // Agregação

    public Time(String nomeTime) {
        this.nomeTime = nomeTime;
        this.jogadores = new ArrayList<>();
    }

    // Adicionando jogadores criados externamente
    public void adicionarJogador(Jogador jogador) {
        this.jogadores.add(jogador);
    }

    public void exibirEscalacao() {
        System.out.println("Elenco do " + nomeTime + ":");
        for (Jogador j : jogadores) {
            System.out.println("- " + j.getNome() + " (" + j.getPosicao() + ")");
        }
    }
}

public class ExemploTimeJogador {
    public static void main(String[] args) {
        Jogador j1 = new Jogador("Alisson", "Goleiro");
        Jogador j2 = new Jogador("Casemiro", "Volante");
        Jogador j3 = new Jogador("Vini Jr", "Atacante");

        Time time = new Time("Brasil");
        time.adicionarJogador(j1);
        time.adicionarJogador(j2);
        time.adicionarJogador(j3);

        time.exibirEscalacao();

        // O jogador 'j3' pode ser transferido para outro time sem ser recriado!
    }
}
```

---

## 🎯 Por que preferir Agregação à Herança?

O princípio de design **"Favoreça a Composição/Agregação em vez da Herança"** (*Favor composition over inheritance*) do livro *Design Patterns (GoF)* recomenda agregação porque:

1. **Baixo Acoplamento:** Alterações internas na classe agregada não quebram a classe dona.
2. **Flexibilidade Dinâmica:** O objeto agregado pode ser trocado em tempo de execução via método `setter`.
3. **Evita Hierarquias Rígidas:** Não força uma classe a herdar métodos que ela não precisa.
4. **Facilidade de Testes Unitários:** Permite injetar facilmente objetos falsos (*Mocks*) durante testes automatizados.

---

## 📋 Resumo Rápido

| Característica | Detalhe |
| :--- | :--- |
| **Tipo de Relação** | `HAS-A` ("TEM-UM") Fraco |
| **Ciclo de Vida** | Independente (o objeto agregado sobrevive à destruição do objeto dono) |
| **Passagem de Instância** | Via Construtor ou Métodos Setters (*Injeção de Dependência*) |
| **Direção** | Pode ser unidirecional ou bidirecional |
| **Vantagem Principal** | Reutilização de objetos existentes sem forte dependência de acoplamento |

---

[⬅️ Voltar ao Índice Principal](../README.md)
