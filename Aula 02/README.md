# ☕ Aula 02 — Assertions Avançadas & @ParameterizedTest

> Desenvolvimento de Sistemas · Testes em Java · Bloco 1 — Fundamentos de JUnit 5
> Duração: 4h · `@karizeviecelli · 2026`

Este arquivo reúne **todo o conteúdo da Aula 02** em texto, para você acompanhar antes, durante ou depois da aula. As versões interativas estão na mesma pasta:

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

### 1. Assertions Avançadas & @ParameterizedTest

Um teste por valor é bom. Um teste que cobre dezenas de valores automaticamente, sem repetir código, é melhor. Hoje você aprende a multiplicar a cobertura sem multiplicar o código.

### 2. O problema de testar caso por caso

> 🏭 Um fiscal de qualidade não testa uma única peça da linha de produção e declara tudo aprovado — ele testa uma **amostra representativa**. Na Aula 01 você escreveu um teste por cenário. E se fossem 20 cenários parecidos, só mudando os números?

Copiar e colar o mesmo teste 20 vezes, trocando só o valor, é sinal de que existe uma ferramenta melhor: o teste parametrizado.

### 3. O que é @ParameterizedTest?

Com `@ParameterizedTest`, o mesmo método de teste roda **várias vezes**, cada execução recebendo um conjunto diferente de dados de entrada — e cada uma aparece separada no relatório.

- 🔁 Troca `@Test` por `@ParameterizedTest`.
- 📦 Uma anotação de **fonte de dados** (`@ValueSource`, `@CsvSource`, `@MethodSource`...) alimenta o método.
- 📊 O relatório mostra cada execução separadamente, não como um teste só.

### 4. @ValueSource — uma lista simples

```java
@ParameterizedTest
@ValueSource(ints = {2, 4, 6, 8})
void deveSerPar(int numero) {
    assertTrue(numero % 2 == 0);
}
```

> 📋 É a forma mais simples: uma lista de valores do mesmo tipo (`int`, `String`, `double`...), cada um vira uma execução do teste.

### 5. @CsvSource — entrada e saída juntas

```java
@ParameterizedTest
@CsvSource({"0, 32", "100, 212"})
void deveConverter(double celsius, double fahrenheit) {
    assertEquals(fahrenheit, conversor.paraF(celsius), 0.01);
}
```

> 💱 É como consultar uma tabela de câmbio já pronta: cada linha do texto CSV traz o valor de entrada **e** o valor de saída esperado, prontos para comparar.

### 6. @MethodSource — dados complexos

Quando os dados de teste são complexos demais para caber numa anotação, um método Java monta a lista para você — como um boletim meteorológico com várias cidades e suas categorias de clima.

```java
@ParameterizedTest
@MethodSource("casos")
void deveClassificar(double valor, String esperado) { ... }

static Stream<Arguments> casos() {
    return Stream.of(Arguments.of(-5.0, "Frio"));
}
```

### 7. assertEquals com margem de erro

> ⚖️ Assim como a balança da padaria aceita uma variação de poucos gramas, números decimais (`double`) quase nunca batem exatamente. O terceiro parâmetro de `assertEquals` define a margem de erro aceitável.

```java
assertEquals(97.88, resultado, 0.01);
```

### 8. Nomeando cada execução

- 🏷️ `@ParameterizedTest(name = "{0}°C vira {1}°F")` troca o nome genérico por um nome específico por execução.
- 🔢 `{0}`, `{1}` referem-se aos parâmetros; `{index}` é o número da execução.
- 📈 Num relatório com 20 execuções, isso é a diferença entre "achar a agulha no palheiro" e ver o problema na hora.

### 9. O que vem a seguir

Agora que você conhece `@ValueSource`, `@CsvSource`, `@MethodSource` e assertions com margem de erro, é hora de ver tudo isso rodando de verdade, com vários exemplos comentados → **Demonstração**.

---

## 💻 Demonstração

> Versão interativa com digitação animada e console simulado: [`02_demo.html`](./02_demo.html)
> Todos os exemplos usam a mesma classe de produção: `ConversorTemperatura`.

### Exemplo 1 — Ida e volta (`@ParameterizedTest` + `@ValueSource`)

> 💱 É como converter reais para dólares e depois de volta para reais: o valor final deveria ficar praticamente igual ao original. `@ValueSource` alimenta o mesmo teste com vários valores de entrada, um de cada vez.

```java
class ConversorTemperaturaTest {

    ConversorTemperatura c = new ConversorTemperatura();

    @ParameterizedTest
    @ValueSource(doubles = {0, 37, 100, -10})
    void converterEVoltarDeveDarOMesmoValor(double celsiusOriginal) {
        double fahrenheit = c.celsiusParaFahrenheit(celsiusOriginal);
        double celsiusDeVolta = c.fahrenheitParaCelsius(fahrenheit);

        assertEquals(celsiusOriginal, celsiusDeVolta, 0.001);
    }
}
```

**Saída:** 4 execuções, uma por valor — todas batendo com o original.

### Exemplo 2 — Tabela de conversão (`@CsvSource`)

> 📇 É como consultar uma tabela de câmbio já impressa: cada linha do CSV já traz o valor de entrada e o valor de saída esperado lado a lado.

```java
@ParameterizedTest
@CsvSource({
    "0, 32",
    "100, 212",
    "-40, -40",
    "37, 98.6"
})
void deveConverterCelsiusParaFahrenheit(double celsius, double fahrenheitEsperado) {
    assertEquals(fahrenheitEsperado, c.celsiusParaFahrenheit(celsius), 0.01);
}
```

**Saída:** `✅ 0°C → 32°F` · `✅ 100°C → 212°F` · `✅ -40°C → -40°F` · `✅ 37°C → 98.6°F`

### Exemplo 3 — Dados complexos (`@MethodSource`)

> 🌤️ Quando os dados de teste ficam grandes ou complexos demais para caber numa anotação, um método Java monta a lista pra você — como um boletim meteorológico com várias cidades.

```java
@ParameterizedTest
@MethodSource("climasParaTestar")
void deveClassificarClimaCorretamente(double celsius, String classificacaoEsperada) {
    assertEquals(classificacaoEsperada, c.classificarClima(celsius));
}

static Stream<Arguments> climasParaTestar() {
    return Stream.of(
        Arguments.of(-5.0, "Frio"),
        Arguments.of(18.0, "Ameno"),
        Arguments.of(32.0, "Quente")
    );
}
```

**Saída:** `✅ -5.0°C → "Frio"` · `✅ 18.0°C → "Ameno"` · `✅ 32.0°C → "Quente"`

### Exemplo 4 — Margem de erro (`assertEquals` com delta)

> ⚖️ Assim como a balança da padaria aceita uma variação de poucos gramas, números decimais quase nunca batem na casa exata.

```java
@Test
void deveAceitarPequenaMargemDeArredondamento() {
    double resultado = c.celsiusParaFahrenheit(36.6);
    assertEquals(97.88, resultado, 0.01);
}
```

**Saída:** `✅ esperado 97.88, obtido 97.88 (margem 0.01)`

### Exemplo 5 — Valores impossíveis (`assertThrows` + `@ValueSource`)

> 🚨 É o alarme de segurança de uma caldeira industrial: dispara para qualquer valor fisicamente impossível. Nenhuma temperatura pode existir abaixo do zero absoluto (-273,15°C).

```java
@ParameterizedTest
@ValueSource(doubles = {-300, -500, -273.16})
void deveRecusarTemperaturasAbaixoDoZeroAbsoluto(double celsiusInvalido) {
    assertThrows(
        IllegalArgumentException.class,
        () -> c.celsiusParaFahrenheit(celsiusInvalido)
    );
}
```

**Saída:** os três valores são rejeitados corretamente.

### Exemplo 6 — Nome por execução (`@ParameterizedTest(name = ...)`)

> 🏷️ É a etiqueta personalizada de cada item numa linha de produção, em vez de uma etiqueta genérica repetida.

```java
@ParameterizedTest(name = "{0}°C deve virar aproximadamente {1}°F")
@CsvSource({
    "0, 32",
    "20, 68",
    "100, 212"
})
void deveExibirNomeDescritivoPorCaso(double celsius, double fahrenheitEsperado) {
    assertEquals(fahrenheitEsperado, c.celsiusParaFahrenheit(celsius), 0.01);
}
```

**Saída:** `✅ 0°C deve virar aproximadamente 32°F` · `✅ 20°C deve virar aproximadamente 68°F` · `✅ 100°C deve virar aproximadamente 212°F`

### Exemplo 7 — Tudo junto: conversão + classificação (`assertAll` + `@CsvSource`)

> 🧪 É a inspeção final de qualidade que reúne todas as checagens da linha de produção num único laudo por item.

```java
@ParameterizedTest
@CsvSource({
    "-5, Frio",
    "18, Ameno",
    "32, Quente"
})
void deveConverterEClassificarNoMesmoTeste(double celsius, String classificacaoEsperada) {
    double fahrenheit = c.celsiusParaFahrenheit(celsius);
    String classificacao = c.classificarClima(celsius);

    assertAll("conversão completa",
        () -> assertTrue(fahrenheit > -459.67),
        () -> assertEquals(classificacaoEsperada, classificacao)
    );
}
```

**Saída:** 3 execuções, 6 verificações no total, todas passando.

---

## 🛠 Prática Guiada

> Versão interativa com checklist, dicas e gabarito revelável: [`03_pratica.html`](./03_pratica.html)

Nesta prática você **não** reusa o `ConversorTemperatura` da demonstração. O cenário agora é um **`ValidadorDeCPF`** — atenção: é uma regra didática de formato (não é o algoritmo oficial da Receita Federal, é só um exercício de teste). Siga os passos na ordem, tente escrever sozinho antes de abrir o gabarito.

**Classe de produção disponível:** `ValidadorDeCPF` · método `validar(String cpf)` retorna `boolean` · regra didática: precisa ter exatamente 11 dígitos numéricos, sem letras nem espaços.

### Passo 1 — Preparar a classe de teste

Crie a classe `ValidadorDeCPFTest` com um atributo `validador` e um método anotado com `@BeforeEach` que inicializa o validador.

<details>
<summary>💡 Dica</summary>

Lembre-se do exemplo 1 da demonstração: um atributo de instância criado do zero antes de cada teste.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
class ValidadorDeCPFTest {

    ValidadorDeCPF validador;

    @BeforeEach
    void preparar() {
        validador = new ValidadorDeCPF();
    }
}
```
</details>

### Passo 2 — Testar um CPF válido

Escreva um teste (não parametrizado) que confirma que o CPF `"12345678901"` (11 dígitos) é considerado válido pelo formato.

<details>
<summary>💡 Dica</summary>

Ainda não precisa de `@ParameterizedTest` aqui — é só um `assertTrue` de um único caso, para aquecer.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
@Test
void deveAceitarCpfComOnzeDigitos() {
    assertTrue(validador.validar("12345678901"));
}
```
</details>

### Passo 3 — Testar vários CPFs inválidos

Escreva um teste parametrizado com `@ValueSource` que recebe pelo menos 3 CPFs com formato inválido (menos de 11 dígitos, com letras, com espaços) e confirma que todos retornam `false`.

<details>
<summary>💡 Dica</summary>

Reveja o exemplo 5 da demonstração: `@ValueSource` roda o mesmo teste uma vez para cada valor da lista.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
@ParameterizedTest
@ValueSource(strings = {"123", "1234567890a", "123 456 789 01"})
void deveRecusarCpfComFormatoInvalido(String cpfInvalido) {
    assertFalse(validador.validar(cpfInvalido));
}
```
</details>

### Passo 4 — Testar vários CPFs de uma vez

Escreva um teste parametrizado com `@CsvSource` cobrindo pelo menos 4 casos (2 válidos, 2 inválidos), passando o CPF e o resultado esperado (`true`/`false`) na mesma linha.

<details>
<summary>💡 Dica</summary>

Reveja o exemplo 2 da demonstração: cada linha do CSV já traz entrada e saída esperada juntas.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
@ParameterizedTest
@CsvSource({
    "12345678901, true",
    "98765432100, true",
    "111, false",
    "abcdefghijk, false"
})
void deveValidarVariosCpfsDeUmaVez(String cpf, boolean esperado) {
    assertEquals(esperado, validador.validar(cpf));
}
```
</details>

### Passo 5 — Dar nomes legíveis a cada execução

Adicione um nome descritivo ao teste parametrizado do passo anterior usando `@ParameterizedTest(name = "...")` com os placeholders `{0}` e `{1}`.

<details>
<summary>💡 Dica</summary>

Reveja o exemplo 6 da demonstração — o nome aparece por execução no relatório, não só uma vez para o teste inteiro.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
@ParameterizedTest(name = "CPF \"{0}\" deveria ser válido? {1}")
@CsvSource({
    "12345678901, true",
    "111, false"
})
void deveValidarComNomeDescritivo(String cpf, boolean esperado) {
    assertEquals(esperado, validador.validar(cpf));
}
```
</details>

### Passo 6 — Rodar tudo e conferir

Rode a classe `ValidadorDeCPFTest` completa (`Ctrl+Shift+F10` na IDE, ou `mvn test` no terminal) e confirme que todas as execuções passam, sem alterar nenhum código da classe `ValidadorDeCPF`.

<details>
<summary>💡 Dica</summary>

Se alguma execução falhar, releia o nome gerado por `@ParameterizedTest` — ele já mostra qual valor de entrada quebrou.
</details>

<details>
<summary>📖 Saída esperada</summary>

```
$ mvn test

[INFO] Running ValidadorDeCPFTest
✅ deveAceitarCpfComOnzeDigitos
✅ deveRecusarCpfComFormatoInvalido (3 execuções)
✅ deveValidarVariosCpfsDeUmaVez (4 execuções)
✅ deveValidarComNomeDescritivo (2 execuções)
[INFO] Tests run: 10, Failures: 0
```
</details>

---

## 🏆 Desafio

> Versão interativa com cronômetro e checklist: [`04_desafio.html`](./04_desafio.html)
> ⚠️ Este desafio **não tem gabarito** — é para você aplicar sozinho o que praticou.

### Missão: teste o `ClassificadorDeTriangulo` sozinho

Use o que você praticou com `ConversorTemperatura` (demonstração) e `ValidadorDeCPF` (prática guiada) para testar uma terceira classe: um classificador de triângulos por tipo de lado.

**Classe de produção disponível:** `ClassificadorDeTriangulo` · método `classificar(double a, double b, double c)` retorna `String` ("Equilátero", "Isósceles" ou "Escaleno") · lança `IllegalArgumentException` quando os três lados não formam um triângulo válido.

**Tempo sugerido:** 45 minutos.

### Checklist de requisitos

- [ ] **Estrutura inicial** — Criar `ClassificadorDeTrianguloTest` com `@BeforeEach` inicializando o classificador.
- [ ] **Triângulo equilátero** — Teste parametrizado com pelo menos 3 casos de lados iguais, esperando `"Equilátero"`.
- [ ] **Triângulo isósceles** — Teste parametrizado com pelo menos 3 casos de dois lados iguais, esperando `"Isósceles"`.
- [ ] **Triângulo escaleno** — Teste parametrizado com pelo menos 3 casos de lados diferentes, esperando `"Escaleno"`.
- [ ] **Lados inválidos** — Testar que lados que não formam um triângulo (ex.: 1, 1, 10) lançam `IllegalArgumentException`.
- [ ] **Nome por execução** — Usar `@ParameterizedTest(name = "...")` com placeholder em pelo menos um teste.
- [ ] **Dados via método** — Usar `@MethodSource` em pelo menos um teste, combinando os três tipos numa lista de casos.
- [ ] **Suíte 100% verde** — Rodar todos os testes da classe e confirmar que todos passam.

### Critérios de entrega

- Usa `@ParameterizedTest` (com `@CsvSource`, `@ValueSource` ou `@MethodSource`) em pelo menos 3 dos testes.
- Cobre os três tipos de triângulo (equilátero, isósceles, escaleno) e o caso inválido.
- Nomes de método e/ou `name =` deixam claro o que cada execução está verificando.
- A suíte roda 100% verde, sem alterar a classe `ClassificadorDeTriangulo`.

Depois de concluir, seu professor vai registrar a avaliação na página de **Feedback** (`05_feedback.html`).

---

*Aula 02 · Testes Java · @karizeviecelli · 2026*
