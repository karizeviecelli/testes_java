# ☕ Aula 01 — JUnit 5 & Primeiro Teste

> Desenvolvimento de Sistemas · Testes em Java · Bloco 1 — Fundamentos de JUnit 5
> Duração: 4h · `@karizeviecelli · 2026`

Este arquivo reúne **todo o conteúdo da Aula 01** em texto, para você acompanhar antes, durante ou depois da aula. As versões interativas (slides navegáveis, editor com digitação animada, checklist clicável, cronômetro) estão na mesma pasta:

| Página | Arquivo interativo |
|---|---|
| 📖 Exposição | `01_exposicao.html` |
| 💻 Demonstração | `02_demo.html` |
| 🛠 Prática Guiada | `03_pratica.html` |
| 🏆 Desafio | `04_desafio.html` |
| 📊 Feedback (professor) | `05_feedback.html` |

## Sumário

- [📖 Exposição](#-exposição)
- [💻 Demonstração](#-demonstração)
- [🛠 Prática Guiada](#-prática-guiada)
- [🏆 Desafio](#-desafio)

---

## 📖 Exposição

> Versão interativa com slides navegáveis: [`01_exposicao.html`](./01_exposicao.html)

### 1. JUnit 5 & o Primeiro Teste

Hoje você vai deixar de confiar "de olho" que seu código funciona e passar a **provar isso**, de forma automática e repetível.

### 2. Por que testar automaticamente?

> 🍞 Pense num padeiro que, antes de vender o pão, sempre corta uma fatia da primeira fornada para provar. Ele não confia "no olho" — ele **verifica**. Um teste automatizado é essa fatia: uma verificação rápida e repetível, toda vez que o "forno" (seu código) faz uma nova fornada.

- 🐢 **Teste manual** é lento, cansativo e você esquece de repetir depois de cada mudança.
- 🤖 **Teste automatizado** roda em segundos, sempre do mesmo jeito, e avisa exatamente o que quebrou.
- 🛡️ Funciona como uma **rede de segurança**: você refatora sem medo de quebrar algo que já funcionava.

### 3. O que é o JUnit 5?

JUnit 5 é o framework padrão de testes para Java. Ele é dividido em três partes que trabalham juntas:

- 🧩 **JUnit Platform** — o "motor" que descobre e executa os testes.
- ⭐ **JUnit Jupiter** — a API que você usa no dia a dia: `@Test`, `assertEquals`, etc.
- 🕰️ **JUnit Vintage** — camada de compatibilidade para rodar testes antigos (JUnit 3/4).

### 4. Anatomia de um teste

```java
@Test
void deveSomarDoisNumeros() {
    Calculadora c = new Calculadora();
    int resultado = c.somar(2, 3);
    assertEquals(5, resultado);
}
```

> 🔎 A anotação `@Test` é como uma etiqueta de "isto aqui é um caso de verificação" — o JUnit procura por ela para saber o que precisa executar.

### 5. O padrão AAA

- 🧾 **Arrange** — separar os ingredientes: criar objetos e preparar os dados de entrada.
- 🍳 **Act** — cozinhar: chamar o método que você quer testar.
- 👅 **Assert** — provar o prato: comparar o resultado obtido com o resultado esperado.

Todo teste bem escrito segue essas três etapas, nessa ordem — mesmo que não estejam comentadas no código.

### 6. Assertions básicas

| Assertion | O que faz |
|---|---|
| `assertEquals(esperado, obtido)` | compara dois valores |
| `assertTrue(condição)` / `assertFalse(condição)` | valida uma expressão booleana |
| `assertNotNull(objeto)` | garante que algo foi de fato criado |
| `assertThrows(Exceção.class, () -> ...)` | confirma que um erro esperado realmente acontece |

### 7. @BeforeEach e @AfterEach

> 🧹 É como arrumar a bancada da cozinha **antes** de cada prato e limpar tudo **depois**. Cada teste começa do zero, sem "sujeira" deixada pelo teste anterior.

```java
@BeforeEach
void preparar() {
    calculadora = new Calculadora();
}
```

### 8. Nomeando e organizando testes

- 📛 Nomes descritivos: `deveLancarExcecaoQuandoDivideParZero` em vez de `teste1`.
- 🏷️ `@DisplayName("...")` permite escrever a intenção em linguagem natural.
- 📁 A classe de teste normalmente fica em `src/test/java`, espelhando o pacote da classe original.

### 9. O que vem a seguir

Agora que você conhece a anatomia de um teste, o padrão AAA e o ciclo de vida com JUnit 5, é hora de ver tudo isso rodando de verdade, com vários exemplos comentados → **Demonstração**.

---

## 💻 Demonstração

> Versão interativa com digitação animada e console simulado: [`02_demo.html`](./02_demo.html)
> Todos os exemplos usam a mesma classe de produção: `Calculadora`.

### Exemplo 1 — Primeiro teste (`assertEquals`)

> 🥖 É como conferir o troco na padaria: você sabe quanto deveria receber e confere se o valor bate. `assertEquals` compara o valor esperado com o valor que o código realmente devolveu.

```java
class CalculadoraTest {

    @Test
    void deveSomarDoisNumeros() {
        // Arrange
        Calculadora c = new Calculadora();

        // Act
        int resultado = c.somar(2, 3);

        // Assert
        assertEquals(5, resultado);
    }
}
```

**Saída:** `✅ deveSomarDoisNumeros() — esperado 5, obtido 5`

### Exemplo 2 — Testando exceções (`assertThrows`)

> 🧯 Um teste de exceção é como testar o fusível de segurança de casa: você provoca de propósito uma situação de curto-circuito (dividir por zero) só para confirmar que a proteção realmente desarma.

```java
@Test
void deveLancarExcecaoAoDividirPorZero() {
    Calculadora c = new Calculadora();

    ArithmeticException erro = assertThrows(
        ArithmeticException.class,
        () -> c.dividir(10, 0)
    );

    assertEquals("/ by zero", erro.getMessage());
}
```

**Saída:** `✅ exceção ArithmeticException capturada` · `✅ mensagem confere: "/ by zero"`

### Exemplo 3 — Preparando o cenário (`@BeforeEach`)

> 🍳 É o *mise en place* da cozinha: antes de cada prato (cada teste), a bancada é arrumada do zero. `@BeforeEach` roda antes de todo teste, garantindo que nenhum objeto de um teste "vaze" para o próximo.

```java
class CalculadoraTest {

    Calculadora calculadora;

    @BeforeEach
    void preparar() {
        calculadora = new Calculadora();
    }

    @Test
    void deveMultiplicarDoisNumeros() {
        int resultado = calculadora.multiplicar(4, 5);
        assertEquals(20, resultado);
    }
}
```

**Saída:** `🧹 preparar() executado` · `✅ esperado 20, obtido 20`

### Exemplo 4 — Vários checks de uma vez (`assertAll`)

> 🚗 Pense na inspeção veicular anual: o mecânico não para no primeiro item reprovado, ele confere freios, luzes e pneus e te entrega o laudo completo. `assertAll` faz o mesmo: roda todas as verificações e mostra tudo que falhou, não só a primeira.

```java
@Test
void deveValidarVariasOperacoesAoMesmoTempo() {
    Calculadora c = new Calculadora();

    assertAll("operações básicas",
        () -> assertEquals(7, c.somar(3, 4)),
        () -> assertEquals(1, c.subtrair(4, 3)),
        () -> assertEquals(12, c.multiplicar(3, 4))
    );
}
```

**Saída:** as três verificações passam de forma independente.

### Exemplo 5 — Igualdade x identidade (`assertEquals` vs `assertSame`)

> 👯 Dois gêmeos idênticos parecem exatamente iguais, mas são duas pessoas diferentes. `assertEquals` compara a "aparência" (o conteúdo); `assertSame`/`assertNotSame` compara se é literalmente o mesmo objeto na memória.

```java
@Test
void deveDiferenciarIgualdadeDeIdentidade() {
    String a = new String("junit");
    String b = new String("junit");

    assertEquals(a, b);
    assertNotSame(a, b);
}
```

**Saída:** `✅ mesmo conteúdo "junit"` · `✅ objetos distintos na memória`

### Exemplo 6 — Nomes legíveis (`@DisplayName`)

> 🏷️ É a etiqueta de prateleira do supermercado: em vez de um código interno, o cliente lê "Arroz integral 1kg". `@DisplayName` troca o nome técnico do método por uma frase que qualquer pessoa da equipe entende no relatório.

```java
@Test
@DisplayName("Deve retornar zero ao somar dois números opostos")
void testeSomaOpostos() {
    Calculadora c = new Calculadora();
    assertEquals(0, c.somar(5, -5));
}
```

**Saída:** `✅ Deve retornar zero ao somar dois números opostos`

### Exemplo 7 — Tudo junto: padrão AAA (cenário completo)

> 🎂 Uma receita de bolo tem ordem: separar os ingredientes, misturar e assar, depois provar. Esse exemplo junta tudo que vimos — Arrange, Act, Assert — num teste só, incluindo uma margem de erro para números decimais.

```java
@Test
void deveCalcularMediaDeNotas() {
    // Arrange
    Calculadora c = new Calculadora();
    double[] notas = {7.5, 8.0, 9.0};

    // Act
    double media = c.calcularMedia(notas);

    // Assert
    assertEquals(8.16, media, 0.01);
}
```

**Saída:** `✅ esperado 8.16, obtido 8.17 (margem 0.01)`

---

## 🛠 Prática Guiada

> Versão interativa com checklist, dicas e gabarito revelável: [`03_pratica.html`](./03_pratica.html)

Nesta prática você **não** reusa a `Calculadora` da demonstração. O cenário agora é uma **`ContaBancaria`**: depósito, saque e uma regra de negócio nova — saldo insuficiente. Siga os passos na ordem, tente escrever sozinho antes de abrir o gabarito.

**Classe de produção disponível:** `ContaBancaria(double saldoInicial)` · `depositar(double v)` · `sacar(double v)` · `consultarSaldo()` · lança `SaldoInsuficienteException`

### Passo 1 — Preparar a classe de teste

Crie a classe `ContaBancariaTest` com um atributo `conta` e um método anotado com `@BeforeEach` que inicializa uma conta com saldo inicial de **R$ 100,00**.

<details>
<summary>💡 Dica</summary>

Lembre-se do exemplo 3 da demonstração: um atributo de instância criado do zero antes de cada teste, para nenhum teste "herdar" o estado do anterior.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
class ContaBancariaTest {

    ContaBancaria conta;

    @BeforeEach
    void preparar() {
        conta = new ContaBancaria(100.0);
    }
}
```
</details>

### Passo 2 — Testar um depósito

Escreva um teste que deposita **R$ 50,00** na conta e verifica que o saldo passa a ser **R$ 150,00**.

<details>
<summary>💡 Dica</summary>

Siga o padrão AAA: Arrange já está pronto (o `@BeforeEach`), Act é chamar `depositar`, Assert é comparar com `consultarSaldo()`.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
@Test
void deveAumentarSaldoAoDepositar() {
    conta.depositar(50.0);
    assertEquals(150.0, conta.consultarSaldo());
}
```
</details>

### Passo 3 — Testar um saque válido

Escreva um teste que saca **R$ 30,00** e verifica que o saldo passa a ser **R$ 70,00**.

<details>
<summary>💡 Dica</summary>

É o mesmo raciocínio do passo anterior, só que chamando `sacar` em vez de `depositar`.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
@Test
void deveDiminuirSaldoAoSacar() {
    conta.sacar(30.0);
    assertEquals(70.0, conta.consultarSaldo());
}
```
</details>

### Passo 4 — Testar saldo insuficiente

Escreva um teste que tenta sacar **R$ 500,00** de uma conta com apenas R$ 100,00 e verifica que uma `SaldoInsuficienteException` é lançada.

<details>
<summary>💡 Dica</summary>

Reveja o exemplo 2 da demonstração — o mesmo padrão de `assertThrows` usado para a divisão por zero se aplica aqui.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
@Test
void deveLancarExcecaoAoSacarValorMaiorQueSaldo() {
    assertThrows(
        SaldoInsuficienteException.class,
        () -> conta.sacar(500.0)
    );
}
```
</details>

### Passo 5 — Dar nomes legíveis

Escolha pelo menos **dois** dos testes que você já escreveu e adicione `@DisplayName` com uma frase clara, em português, descrevendo o comportamento esperado.

<details>
<summary>💡 Dica</summary>

Reveja o exemplo 6 da demonstração. Uma boa frase responde: "o que deveria acontecer quando...".
</details>

<details>
<summary>📖 Gabarito</summary>

```java
@Test
@DisplayName("Deve recusar saque maior que o saldo disponível")
void deveLancarExcecaoAoSacarValorMaiorQueSaldo() {
    assertThrows(
        SaldoInsuficienteException.class,
        () -> conta.sacar(500.0)
    );
}
```
</details>

### Passo 6 — Rodar tudo e conferir

Rode a classe `ContaBancariaTest` completa (`Ctrl+Shift+F10` na IDE, ou `mvn test` no terminal) e confirme que os 4 testes passam, sem alterar nenhum código da classe `ContaBancaria`.

<details>
<summary>💡 Dica</summary>

Se algum teste falhar, releia a mensagem do JUnit com calma — ela sempre mostra o valor esperado e o valor obtido.
</details>

<details>
<summary>📖 Saída esperada</summary>

```
$ mvn test

[INFO] Running ContaBancariaTest
✅ deveAumentarSaldoAoDepositar
✅ deveDiminuirSaldoAoSacar
✅ Deve recusar saque maior que o saldo disponível
✅ (mais 1 teste)
[INFO] Tests run: 4, Failures: 0
```
</details>

---

## 🏆 Desafio

> Versão interativa com cronômetro e checklist: [`04_desafio.html`](./04_desafio.html)
> ⚠️ Este desafio **não tem gabarito** — é para você aplicar sozinho o que praticou.

### Missão: teste o `ValidadorDeSenha` sozinho

Chegou a hora de aplicar tudo sem apoio. Use o que você praticou com `Calculadora` (demonstração) e `ContaBancaria` (prática guiada) para testar uma terceira classe: um validador de senhas.

**Classe de produção disponível:** `ValidadorDeSenha` · método `validar(String senha)` retorna `boolean` · regras: mínimo 8 caracteres, ao menos 1 número, ao menos 1 letra maiúscula · senha com espaço lança `SenhaComEspacoException`

**Tempo sugerido:** 40 minutos.

### Checklist de requisitos

- [ ] **Estrutura inicial** — Criar `ValidadorDeSenhaTest` com `@BeforeEach` inicializando o validador.
- [ ] **Senha válida** — Testar que uma senha correta (8+ caracteres, número e maiúscula) retorna `true`.
- [ ] **Senha curta** — Testar que uma senha com menos de 8 caracteres retorna `false`.
- [ ] **Sem número** — Testar que uma senha sem nenhum dígito retorna `false`.
- [ ] **Sem letra maiúscula** — Testar que uma senha totalmente minúscula retorna `false`.
- [ ] **Senha com espaço** — Testar que uma senha contendo espaço lança `SenhaComEspacoException` (`assertThrows`).
- [ ] **Nomes descritivos** — Usar `@DisplayName` em pelo menos 2 dos testes escritos.
- [ ] **Suíte 100% verde** — Rodar todos os testes da classe e confirmar que todos passam.

### Critérios de entrega

- Todos os testes seguem o padrão AAA (mesmo sem comentários explícitos).
- Nomes de método e/ou `@DisplayName` deixam claro o que está sendo verificado.
- Casos de borda (senha curta, sem número, sem maiúscula, com espaço) estão cobertos.
- A suíte roda 100% verde, sem alterar a classe `ValidadorDeSenha`.

Depois de concluir, seu professor vai registrar a avaliação na página de **Feedback** (`05_feedback.html`).

---

*Aula 01 · Testes Java · @karizeviecelli · 2026*
