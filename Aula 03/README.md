# ☕ Aula 03 — TDD: Red, Green, Refactor

> Desenvolvimento de Sistemas · Testes em Java · Bloco 1 — Fundamentos de JUnit 5
> Duração: 4h · `@karizeviecelli · 2026`

Este arquivo reúne **todo o conteúdo da Aula 03** em texto, para você acompanhar antes, durante ou depois da aula. As versões interativas estão na mesma pasta:

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

### 1. TDD — Red, Green, Refactor

E se, em vez de escrever o código e depois testar, você escrevesse o teste primeiro — e deixasse ele te guiar até o código certo? Hoje você aprende o ciclo que muda a ordem das coisas.

### 2. Por que escrever o teste primeiro?

> 🧭 É como definir o destino no GPS **antes** de sair dirigindo. O teste é o destino: ele descreve exatamente onde o código precisa chegar, antes de você escrever uma linha sequer de implementação.

Escrever o teste antes força você a pensar no comportamento esperado — não nos detalhes de implementação — antes de codar.

### 3. O ciclo: Red → Green → Refactor

- 🔴 **Red** — escreva um teste para um comportamento que ainda não existe. Ele falha (ou nem compila). Isso é esperado.
- 🟢 **Green** — escreva o código **mínimo** possível para o teste passar. Não precisa ser bonito, só verde.
- 🔵 **Refactor** — melhore o código sem mudar o comportamento. Os testes continuam passando o tempo todo.

E repete: o próximo comportamento vira um novo vermelho.

### 4. Vermelho: a rua que ainda não existe no mapa

```java
@Test
void deveRetornarZeroParaTextoVazio() {
    SomaDeTexto s = new SomaDeTexto();
    assertEquals(0, s.somar(""));
}
```

> 🚧 A classe `SomaDeTexto` nem existe ainda — o teste vai falhar na compilação. É o "vermelho": uma descrição precisa de um destino que o mapa atual não alcança.

### 5. Verde: o mínimo pra sair da garagem

```java
class SomaDeTexto {
    int somar(String texto) {
        return 0;
    }
}
```

> ⛽ Não é colocar tanque cheio — é só o suficiente pra sair da garagem. A implementação mais boba possível já deixa o teste verde, e está tudo bem: ela vai evoluir no próximo ciclo.

### 6. Refactor: reformar com os moradores dentro de casa

> 🏗️ Refatorar é trocar o encanamento de uma casa **sem tirar os moradores dela** e sem derrubar as paredes de sustentação. As paredes de sustentação, aqui, são os testes: se eles continuam verdes, a reforma foi segura.

Nunca refatore sem uma rede de testes cobrindo o comportamento atual — é assim que TDD e testes automatizados se conectam.

### 7. Katas de programação

> 🥋 Assim como um kata de artes marciais é uma sequência de movimentos repetida até virar automática, um "kata de código" é um exercício pequeno (FizzBuzz, String Calculator, contador de vogais...) repetido várias vezes só para treinar o ciclo Red-Green-Refactor.

O objetivo não é o exercício em si — é treinar o hábito de sempre escrever o teste primeiro.

### 8. Um commit por cor

- 🔴 Commit no vermelho: `"red: teste para soma de texto vazio"`.
- 🟢 Commit no verde: `"green: implementação mínima"`.
- 🔵 Commit no refactor: `"refactor: extrai parsing para método"`.

Um histórico granular assim facilita reverter exatamente o passo que deu errado — sem precisar desfazer o trabalho inteiro.

### 9. O que vem a seguir

Agora que você conhece o ciclo Red-Green-Refactor e a ideia de katas, é hora de ver um kata completo sendo construído passo a passo, com vários ciclos comentados → **Demonstração**.

---

## 💻 Demonstração

> Versão interativa com digitação animada e console simulado: [`02_demo.html`](./02_demo.html)
> Os 7 exemplos constroem, ciclo a ciclo, a mesma classe: `SomaDeTexto` (um kata clássico conhecido como "String Calculator").

### Ciclo 1 — Vermelho: o destino que não existe

> 🧭 É como digitar um endereço no GPS antes de ligar o carro: a classe `SomaDeTexto` nem existe ainda, então o teste sequer compila.

```java
class SomaDeTextoTest {

    @Test
    void deveRetornarZeroParaTextoVazio() {
        SomaDeTexto s = new SomaDeTexto();
        assertEquals(0, s.somar(""));
    }
}
```

**Saída:** `❌ erro de compilação: classe SomaDeTexto não existe` — vermelho esperado.

### Ciclo 2 — Verde: o mínimo pra sair da garagem

> ⛽ Retornar 0 sempre já deixa o teste verde. Feio? Talvez. Funciona pro caso atual? Sim.

```java
class SomaDeTexto {

    int somar(String texto) {
        return 0;
    }
}
```

**Saída:** `✅ deveRetornarZeroParaTextoVazio — esperado 0, obtido 0`

### Ciclo 3 — Vermelho de novo: uma rua nova no mapa

> 🚧 Um novo teste é uma rua que o carro atual ainda não sabe percorrer.

```java
@Test
void deveRetornarOProprioNumeroParaUmValor() {
    SomaDeTexto s = new SomaDeTexto();
    assertEquals(1, s.somar("1"));
}
```

**Saída:** `❌ esperado 1, obtido 0` — vermelho esperado.

### Ciclo 4 — Verde de novo: reabastecendo o suficiente

> ⛽ Reabastece só o necessário pra cobrir a rota nova, sem exagerar.

```java
int somar(String texto) {
    if (texto.isEmpty()) return 0;
    return Integer.parseInt(texto);
}
```

**Saída:** os dois testes passam.

### Ciclo 5 — Vermelho: um cruzamento mais complexo

> 🚦 O carro atual só sabe seguir em frente com um número, e agora precisamos somar vários separados por vírgula.

```java
@Test
void deveSomarNumerosSeparadosPorVirgula() {
    SomaDeTexto s = new SomaDeTexto();
    assertEquals(6, s.somar("1,2,3"));
}
```

**Saída:** `❌ NumberFormatException: "1,2,3"` — vermelho esperado.

### Ciclo 6 — Verde: ajustando a rota pro cruzamento

> 🗺️ Generalizar a rota pra cobrir qualquer número de saídas: dividir o texto pela vírgula e somar cada pedaço.

```java
int somar(String texto) {
    if (texto.isEmpty()) return 0;
    String[] partes = texto.split(",");
    int total = 0;
    for (String p : partes) {
        total += Integer.parseInt(p);
    }
    return total;
}
```

**Saída:** os três testes passam.

### Ciclo 7 — Refactor: reforma com os moradores dentro

> 🏗️ O código muda de forma (agora usando streams), mas o comportamento — e os três testes — continuam exatamente os mesmos.

```java
int somar(String texto) {
    if (texto.isEmpty()) return 0;

    return Arrays.stream(texto.split(","))
                 .mapToInt(Integer::parseInt)
                 .sum();
}
```

**Saída:** os três testes continuam passando — comportamento idêntico, código mais limpo.

---

## 🛠 Prática Guiada

> Versão interativa com checklist, dicas e gabarito revelável: [`03_pratica.html`](./03_pratica.html)

Nesta prática você vai construir a classe **`FizzBuzz`** do zero, ciclo por ciclo: primeiro o teste vermelho, depois o código verde mínimo. Não existe uma classe de produção pronta — ela nasce dos testes.

**Regra do kata:** `calcular(int n)` retorna `String` — múltiplos de 3 viram `"Fizz"`, múltiplos de 5 viram `"Buzz"`, múltiplos de 3 e 5 viram `"FizzBuzz"`, os demais retornam o próprio número como texto.

### Ciclo 1 — número comum (Red + Green)

Escreva o teste **vermelho** para um número comum, ex.: `calcular(1)` deve retornar `"1"`. Depois escreva o código **verde mínimo** que faz esse teste passar.

<details>
<summary>💡 Dica</summary>

Verde mínimo aqui pode ser simplesmente devolver o número como texto, sem nenhuma regra de Fizz/Buzz ainda.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
// Teste (vermelho primeiro)
class FizzBuzzTest {

    FizzBuzz fb;

    @BeforeEach
    void preparar() {
        fb = new FizzBuzz();
    }

    @Test
    void deveRetornarONumeroComoTexto() {
        assertEquals("1", fb.calcular(1));
    }
}

// Produção (verde mínimo)
class FizzBuzz {
    String calcular(int n) {
        return String.valueOf(n);
    }
}
```
</details>

### Ciclo 2 — múltiplos de 3 (Red + Green)

Escreva o teste **vermelho** para um múltiplo de 3, ex.: `calcular(3)` deve retornar `"Fizz"`. Depois evolua o código pra ficar **verde** de novo, sem quebrar o teste anterior.

<details>
<summary>💡 Dica</summary>

O jeito mais simples é um `if` novo antes do `return` genérico — ainda não precisa ser elegante.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
@Test
void deveRetornarFizzParaMultiplosDeTres() {
    assertEquals("Fizz", fb.calcular(3));
}

// Produção
String calcular(int n) {
    if (n % 3 == 0) return "Fizz";
    return String.valueOf(n);
}
```
</details>

### Ciclo 3 — múltiplos de 5 (Red + Green)

Escreva o teste **vermelho** para um múltiplo de 5, ex.: `calcular(5)` deve retornar `"Buzz"`. Evolua o código de novo.

<details>
<summary>💡 Dica</summary>

Mais um `if`, seguindo o mesmo padrão do ciclo anterior.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
@Test
void deveRetornarBuzzParaMultiplosDeCinco() {
    assertEquals("Buzz", fb.calcular(5));
}

// Produção
String calcular(int n) {
    if (n % 3 == 0) return "Fizz";
    if (n % 5 == 0) return "Buzz";
    return String.valueOf(n);
}
```
</details>

### Ciclo 4 — múltiplos de 3 e 5 (Red + Green)

Escreva o teste **vermelho** para um múltiplo de 3 <u>e</u> 5 ao mesmo tempo, ex.: `calcular(15)` deve retornar `"FizzBuzz"`. Cuidado com a ordem dos `if` na hora do verde.

<details>
<summary>💡 Dica</summary>

Se você testar "múltiplo de 3" antes de "múltiplo de 15", o 15 vai cair no Fizz e nunca chegar no FizzBuzz. A ordem dos ifs importa.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
@Test
void deveRetornarFizzBuzzParaMultiplosDeTresECinco() {
    assertEquals("FizzBuzz", fb.calcular(15));
}

// Produção
String calcular(int n) {
    if (n % 15 == 0) return "FizzBuzz";
    if (n % 3 == 0) return "Fizz";
    if (n % 5 == 0) return "Buzz";
    return String.valueOf(n);
}
```
</details>

### Ciclo 5 — Refactor: consolidar com @ParameterizedTest

Sem mudar o comportamento, junte os 4 testes separados num único teste parametrizado com `@CsvSource`, cobrindo os 4 casos que você já escreveu.

<details>
<summary>💡 Dica</summary>

Isso é refactor no teste, não na produção — o objetivo é o mesmo: simplificar sem quebrar nada.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
@ParameterizedTest
@CsvSource({
    "1, 1",
    "3, Fizz",
    "5, Buzz",
    "15, FizzBuzz"
})
void deveCalcularFizzBuzzCorretamente(int numero, String esperado) {
    assertEquals(esperado, fb.calcular(numero));
}
```
</details>

### Rodar tudo e conferir

Rode a classe `FizzBuzzTest` completa e confirme que tudo passa, sem alterar o comportamento da classe `FizzBuzz` — só o teste foi refatorado no ciclo anterior.

<details>
<summary>💡 Dica</summary>

Se algo falhar depois do refactor do teste, o comportamento da produção não mudou — o bug está na forma como o novo teste parametrizado foi escrito.
</details>

<details>
<summary>📖 Saída esperada</summary>

```
$ mvn test

[INFO] Running FizzBuzzTest
✅ deveCalcularFizzBuzzCorretamente (4 execuções)
[INFO] Tests run: 4, Failures: 0
```
</details>

---

## 🏆 Desafio

> Versão interativa com cronômetro e checklist: [`04_desafio.html`](./04_desafio.html)
> ⚠️ Este desafio **não tem gabarito** — é para você aplicar sozinho o que praticou.

### Missão: construa o ContadorDeVogais do zero, em TDD

Diferente das últimas missões, **não existe nenhuma classe de produção pronta** — você vai criá-la sozinho, em pequenos ciclos vermelho-verde-refactor, movido pelos testes, como fez com `SomaDeTexto` (demonstração) e `FizzBuzz` (prática guiada).

**Objetivo final:** uma classe `ContadorDeVogais` com um método `contar(String texto)` que retorna `int` — a quantidade de vogais (a, e, i, o, u) na string, ignorando maiúsculas/minúsculas.

**Tempo sugerido:** 50 minutos.

### Checklist de requisitos

- [ ] **Ciclo 1 — string vazia** — Vermelho: `contar("")` deve retornar 0, antes da classe existir. Verde: implementação mínima.
- [ ] **Ciclo 2 — sem vogais** — Vermelho: uma palavra sem vogais (ex.: "pfft") deve retornar 0. Verde: evoluir a implementação.
- [ ] **Ciclo 3 — vogais repetidas** — Vermelho: uma palavra só de vogais (ex.: "aeiou") deve retornar 5. Verde: generalizar a contagem.
- [ ] **Ciclo 4 — maiúsculas e minúsculas** — Vermelho: uma palavra com vogais maiúsculas (ex.: "AEIOU") também deve retornar 5.
- [ ] **Ciclo 5 — palavra comum** — Vermelho: uma palavra do dia a dia (ex.: "programando") com o número certo de vogais contado manualmente.
- [ ] **Refactor real** — Reescrever a implementação (ex.: usando stream ou regex) sem quebrar nenhum teste já escrito.
- [ ] **Nomes descritivos** — Cada teste tem um nome que deixa claro qual comportamento está sendo verificado.
- [ ] **Suíte 100% verde** — Rodar todos os testes da classe e confirmar que todos passam ao final.

### Critérios de entrega

- Cada novo comportamento começou com um teste **vermelho**, escrito antes do código de produção.
- Existe pelo menos um **refactor** real: a implementação mudou de forma, mas os testes continuaram passando sem alteração.
- Nomes de teste descrevem claramente o comportamento verificado.
- A suíte roda 100% verde ao final, cobrindo string vazia, sem vogais e com vogais repetidas.

Depois de concluir, seu professor vai registrar a avaliação na página de **Feedback** (`05_feedback.html`).

---

*Aula 03 · Testes Java · @karizeviecelli · 2026*
