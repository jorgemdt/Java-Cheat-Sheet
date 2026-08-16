# 🔀 Sobrecarga de Métodos (Method Overloading) em Java

A **Sobrecarga de Métodos** (*Method Overloading*) ocorre quando dois ou mais métodos na mesma classe compartilham o **mesmo nome**, mas possuem **listas de parâmetros diferentes** (quantidade, tipos ou ordem dos tipos).

É uma forma de **polimorfismo em tempo de compilação** (polimorfismo estático).

---

## 🎯 Por que usar Sobrecarga?

Permite que uma mesma ação seja executada com diferentes entradas contextuais, sem a necessidade de criar nomes artificiais como `enviarEmail()`, `enviarEmailParaDestinatario()`, `enviarMensagemUrgente()`. Usamos apenas `enviar()`.

---

## 📐 O que compõe a Assinatura de um Método?

Em Java, o compilador diferencia métodos exclusivamente pela sua **assinatura**:

$$\text{Assinatura} = \text{Nome do Método} + \text{Lista de Tipos de Parâmetros}$$

> [!IMPORTANT]
> O **tipo de retorno NÃO faz parte da assinatura**. Mudar apenas o tipo de retorno **NÃO** é sobrecarga e resulta em erro de compilação!

```java
// ❌ ERRO DE COMPILAÇÃO: O compilador não sabe qual executar se apenas o retorno mudar
public boolean autenticar(String token) { return true; }
public String autenticar(String token) { return "OK"; } // Erro: método duplicado!
```

---

## 🛠️ Formas de Realizar a Sobrecarga

Para ser uma sobrecarga válida, a lista de parâmetros deve variar em pelo menos um destes 3 aspectos:

### 1. Variação na **Quantidade** de Parâmetros

```java
public class Notificador {

    // 1 parâmetro: Mensagem geral
    public void enviar(String mensagem) {
        System.out.println("Aviso geral: " + mensagem);
    }

    // 2 parâmetros: Destinatário específico + Mensagem
    public void enviar(String destinatario, String mensagem) {
        System.out.println("Para " + destinatario + ": " + mensagem);
    }

    // 3 parâmetros: Destinatário + Mensagem + Flag de urgência
    public void enviar(String destinatario, String mensagem, boolean urgente) {
        String tag = urgente ? "[URGENTE] " : "[INFO] ";
        System.out.println(tag + "Para " + destinatario + ": " + mensagem);
    }
}
```

---

### 2. Variação nos **Tipos** de Parâmetros

```java
public class SistemaLogin {

    // Login com E-mail e Senha (String, String)
    public void login(String email, String senha) {
        System.out.println("Autenticando via e-mail: " + email);
    }

    // Login com ID numérico e PIN (long, int)
    public void login(long idUsuario, int pin) {
        System.out.println("Autenticando ID " + idUsuario + " com PIN de segurança.");
    }

    // Login apenas com Token de acesso (String única)
    public void login(String tokenAcesso) {
        System.out.println("Autenticando via Token OAuth.");
    }
}
```

---

### 3. Variação na **Ordem** dos Tipos de Parâmetros

```java
public class FormatadorLog {

    // Ordem: int antes de String
    public void registrar(int codigoStatus, String mensagem) {
        System.out.println("Status [" + codigoStatus + "] -> " + mensagem);
    }

    // Ordem: String antes de int
    public void registrar(String mensagem, int codigoStatus) {
        System.out.println(mensagem + " (Código retornado: " + codigoStatus + ")");
    }
}
```

---

## 🏗️ Sobrecarga de Construtores (*Constructor Overloading*)

Assim como métodos comuns, **construtores** também podem ser sobrecarregados para permitir criar objetos de maneiras diferentes (com valores padrão, parciais ou completos).

Podemos reutilizar construtores utilizando `this(...)`:

```java
public class Usuario {
    private String nome;
    private String email;
    private String perfil;

    // Construtor 1: Completo
    public Usuario(String nome, String email, String perfil) {
        this.nome = nome;
        this.email = email;
        this.perfil = perfil;
    }

    // Construtor 2: Perfil padrão de "CLIENTE" (chama o construtor 1)
    public Usuario(String nome, String email) {
        this(nome, email, "CLIENTE");
    }

    // Construtor 3: Usuário Convidado/Anônimo
    public Usuario() {
        this("Convidado", "anonimo@sistema.com", "VISITANTE");
    }
}
```

---

## 🔍 Resolução do Compilador (Type Promotion & Prioridades)

Quando um método sobrecarregado é chamado, o compilador Java escolhe a versão mais específica seguindo a seguinte ordem de prioridade:

1. **Correspondência Exata:** Tipo exatamente igual.
2. **Promoção de Tipo Primitivo (*Widening*):** `byte` $\rightarrow$ `short` $\rightarrow$ `int` $\rightarrow$ `long` $\rightarrow$ `float` $\rightarrow$ `double`.
3. **Autoboxing / Unboxing:** `int` $\rightarrow$ `Integer`.
4. **Varargs:** `tipo...` (é sempre a **última prioridade** na escolha).

### Exemplo de Type Promotion:

```java
public class TestePromocao {
    public static void exibir(int n) {
        System.out.println("int: " + n);
    }

    public static void exibir(double d) {
        System.out.println("double: " + d);
    }

    public static void main(String[] args) {
        short x = 10;
        exibir(x); // Executa exibir(int) porque short é promovido para int antes de double!
    }
}
```

---

## ⚠️ Armadilha: Chamada Ambígua (*Ambiguous Method Call*)

Cuidado ao misturar promoção de tipos e múltiplos argumentos:

```java
public class Ambiguidade {
    public static void demo(int a, double b) { }
    public static void demo(double a, int b) { }

    public static void main(String[] args) {
        // demo(10, 20); // ERRO DE COMPILAÇÃO: Chamada ambígua!
        // O compilador não sabe qual parâmetro promover para double.
    }
}
```

---

## 📋 Resumo Rápido

| Aspecto | É permitido para sobrecarga? |
| :--- | :--- |
| Mudar a **quantidade** de parâmetros | ✅ Sim |
| Mudar os **tipos** de parâmetros | ✅ Sim |
| Mudar a **ordem** dos tipos de parâmetros | ✅ Sim |
| Mudar apenas o **nome dos parâmetros** | ❌ Não (Erro de compilação) |
| Mudar apenas o **tipo de retorno** | ❌ Não (Erro de compilação) |
| Mudar apenas o **modificador de acesso** (`public`, `private`) | ❌ Não (Erro de compilação) |

---

[⬅️ Voltar ao Índice Principal](../README.md)
