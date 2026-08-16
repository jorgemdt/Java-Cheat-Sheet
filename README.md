# ☕ Java Cheat Sheet

Um guia prático, direto ao ponto e estruturado para consulta rápida de sintaxe, conceitos fundamentais e recursos avançados da linguagem **Java**.

## 💡 Como Utilizar

- Cada item do índice abaixo contém um **resumo direto** com **código de exemplo**.
- Clique no link de cada tópico para acessar a **explicação detalhada e completa** na pasta [`topicos/`](topicos/).

---

## 📌 Índice de Tópicos

### - [ ] [01. Varargs (Variable Arguments)](topicos/01-varargs.md)
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
