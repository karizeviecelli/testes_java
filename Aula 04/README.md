# ☕ Aula 04 — Mockito: Mocks e Stubs

> Desenvolvimento de Sistemas · Testes em Java · Bloco 2 — Mocks e Integração
> Duração: 4h · `@karizeviecelli · 2026`

Este arquivo reúne **todo o conteúdo da Aula 04** em texto, para você acompanhar antes, durante ou depois da aula. As versões interativas estão na mesma pasta:

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

### 1. Mockito — Mocks e Stubs

Nem toda classe vive sozinha. Quando o código depende de um banco de dados, uma API externa ou um serviço de pagamento, como testar sem esperar, sem gastar e sem depender de internet?

### 2. O problema das dependências reais

> 🎬 Num filme, o ator não pula de um prédio de verdade — um **dublê** faz a cena de risco no lugar dele. Um mock é o dublê de uma dependência: ele "veste a roupa" de um gateway de pagamento ou de um banco de dados, sem executar nada real.

- 🐢 Chamar um serviço real em todo teste é **lento** e depende de internet.
- 💸 Cobranças de verdade num gateway de pagamento seriam **caras** — e erradas de se testar assim.
- 🎲 APIs externas podem estar fora do ar, tornando o teste **instável** sem culpa do seu código.

### 3. O que é um mock?

Um mock é um objeto "de mentira" que imita a interface de uma dependência real — mas quem decide como ele se comporta é o próprio teste, não uma implementação de verdade.

```java
@Mock
GatewayDePagamento gateway;
```

### 4. Configurando o dublê: when / thenReturn

> 📜 `when/thenReturn` é o roteiro que você entrega ao dublê antes da cena: "quando te perguntarem X, responda Y". Sem esse roteiro, o mock não sabe o que fazer — ele devolve valores vazios por padrão.

```java
when(gateway.cobrar(150.0)).thenReturn(true);
```

### 5. Confirmando a atuação: verify

> 🎥 `verify` é a diferença entre "o ator sabia a fala" e "o ator realmente disse a fala na cena". Configurar um retorno não prova que o método foi chamado — `verify` confirma a interação de verdade.

```java
verify(gateway).cobrar(150.0);
verify(notificador, never()).enviar(any(), any());
```

### 6. Capturando detalhes: ArgumentCaptor

> 🎙️ É gravar exatamente o que o dublê disse na cena, pra conferir os detalhes depois. Não basta saber que o método foi chamado — às vezes você quer inspecionar **o que** foi passado como argumento.

```java
ArgumentCaptor<String> captor = ArgumentCaptor.forClass(String.class);
verify(notificador).enviar(any(), captor.capture());
assertTrue(captor.getValue().contains("300"));
```

### 7. Vocabulário: Mock x Stub x Spy

- 🎭 **Stub** — um dublê que só devolve respostas fixas, sem se importar em como foi chamado.
- 🕵️ **Mock** — além de responder, permite `verify` pra confirmar as interações.
- 👤 **Spy** — envolve um objeto **real**, deixando o comportamento original funcionar, exceto onde você sobrescreve.

No dia a dia, a maioria dos casos usa mocks do Mockito — que cobrem tanto o papel de stub quanto de mock verdadeiro.

### 8. Montagem: @ExtendWith e @InjectMocks

```java
@ExtendWith(MockitoExtension.class)
class ServicoDePedidoTest {

    @Mock
    GatewayDePagamento gateway;

    @InjectMocks
    ServicoDePedido servico;
}
```

`@ExtendWith(MockitoExtension.class)` liga o motor do Mockito nos testes; `@InjectMocks` cria a classe real sob teste, injetando os mocks nela automaticamente.

### 9. O que vem a seguir

Agora que você conhece `@Mock`, `when/thenReturn`, `verify` e `ArgumentCaptor`, é hora de ver tudo isso rodando de verdade, num serviço de pedido com pagamento e notificação por e-mail → **Demonstração**.

---

## 💻 Demonstração

> Versão interativa com digitação animada e console simulado: [`02_demo.html`](./02_demo.html)
> Todos os exemplos usam a mesma classe sob teste: `ServicoDePedido`, que depende de `GatewayDePagamento` e `NotificadorEmail` (ambos mockados).

### Exemplo 1 — Criando o dublê (`@Mock` + `@InjectMocks`)

> 🎬 Assim como um dublê veste a roupa do ator pra fazer a cena de risco, `@Mock` cria um objeto que "veste" a interface `GatewayDePagamento` sem executar nenhuma cobrança real.

```java
@ExtendWith(MockitoExtension.class)
class ServicoDePedidoTest {

    @Mock
    GatewayDePagamento gateway;

    @Mock
    NotificadorEmail notificador;

    @InjectMocks
    ServicoDePedido servico;
}
```

**Saída:** mocks criados — ainda não sabem fazer nada, cabe ao teste ensinar.

### Exemplo 2 — Ensinando o dublê (`when` / `thenReturn`)

> 📜 `when/thenReturn` é o roteiro que você entrega ao dublê antes da cena.

```java
@Test
void deveFinalizarPedidoQuandoPagamentoAprovado() {
    when(gateway.cobrar(150.0)).thenReturn(true);

    boolean resultado = servico.finalizar(new Pedido(150.0));

    assertTrue(resultado);
}
```

**Saída:** `✅ finalizar() retornou true`

### Exemplo 3 — Outro roteiro, outro final (falha no pagamento)

> 🚫 O mock não tem opinião própria — só faz exatamente o que o teste manda.

```java
@Test
void naoDeveFinalizarQuandoPagamentoRecusado() {
    when(gateway.cobrar(150.0)).thenReturn(false);

    boolean resultado = servico.finalizar(new Pedido(150.0));

    assertFalse(resultado);
}
```

**Saída:** `✅ finalizar() retornou false`

### Exemplo 4 — Confirmando a atuação (`verify`)

> 🎥 Aqui confirmamos que o método foi mesmo chamado, com o valor certo.

```java
@Test
void deveChamarGatewayComValorCorreto() {
    when(gateway.cobrar(anyDouble())).thenReturn(true);

    servico.finalizar(new Pedido(200.0));

    verify(gateway).cobrar(200.0);
}
```

**Saída:** `✅ verify: gateway.cobrar(200.0) foi chamado exatamente 1 vez`

### Exemplo 5 — Confirmando que NÃO aconteceu (`verify(never())`)

> 🙅 O dublê do e-mail nunca deveria ter sido chamado quando o pagamento falha.

```java
@Test
void naoDeveEnviarEmailQuandoPagamentoFalha() {
    when(gateway.cobrar(anyDouble())).thenReturn(false);

    servico.finalizar(new Pedido(80.0));

    verify(notificador, never()).enviar(anyString(), anyString());
}
```

**Saída:** `✅ verify: notificador.enviar(...) nunca foi chamado`

### Exemplo 6 — Gravando os detalhes (`ArgumentCaptor`)

> 🎙️ Não basta saber que o e-mail foi enviado — aqui a gente confere o conteúdo exato da mensagem.

```java
@Test
void deveEnviarEmailComMensagemCorreta() {
    when(gateway.cobrar(anyDouble())).thenReturn(true);
    ArgumentCaptor<String> mensagemCaptor = ArgumentCaptor.forClass(String.class);

    servico.finalizar(new Pedido(300.0));

    verify(notificador).enviar(anyString(), mensagemCaptor.capture());
    assertTrue(mensagemCaptor.getValue().contains("300"));
}
```

**Saída:** `✅ a mensagem capturada contém o valor esperado`

### Exemplo 7 — Ensaio geral: tudo junto

> 🎞️ Roteiro combinado (when), atuação conferida (verify) e o resultado final checado (assert) — tudo no mesmo teste.

```java
@Test
void deveExecutarFluxoCompletoDoPedidoAprovado() {
    when(gateway.cobrar(500.0)).thenReturn(true);

    boolean resultado = servico.finalizar(new Pedido(500.0));

    assertAll("fluxo completo",
        () -> assertTrue(resultado),
        () -> verify(gateway).cobrar(500.0),
        () -> verify(notificador).enviar(anyString(), contains("500"))
    );
}
```

**Saída:** as três verificações passam no mesmo teste.

---

## 🛠 Prática Guiada

> Versão interativa com checklist, dicas e gabarito revelável: [`03_pratica.html`](./03_pratica.html)

Nesta prática você **não** reusa o `ServicoDePedido` da demonstração. O cenário agora é um **`ServicoDeEstoque`** que depende de um **`RepositorioProduto`** mockado — sem banco de dados de verdade.

**Classe sob teste:** `ServicoDeEstoque` (depende de `RepositorioProduto`) · método `reservar(String produtoId, int quantidade)` retorna `boolean` · lança `ProdutoNaoEncontradoException` se o produto não existir.

### Passo 1 — Preparar os mocks

Crie a classe `ServicoDeEstoqueTest` anotada com `@ExtendWith(MockitoExtension.class)`, com um `@Mock RepositorioProduto repositorio` e um `@InjectMocks ServicoDeEstoque servico`.

<details>
<summary>💡 Dica</summary>

Lembre-se do exemplo 1 da demonstração: `@Mock` cria o dublê, `@InjectMocks` monta a classe real usando esse dublê.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
@ExtendWith(MockitoExtension.class)
class ServicoDeEstoqueTest {

    @Mock
    RepositorioProduto repositorio;

    @InjectMocks
    ServicoDeEstoque servico;
}
```
</details>

### Passo 2 — Reserva com estoque suficiente

Configure o mock pra devolver um `Produto` com 10 unidades em estoque, e escreva um teste que reserva 3 unidades — deve retornar `true`.

<details>
<summary>💡 Dica</summary>

Siga o padrão do exemplo 2 da demonstração: `when(repositorio.buscarPorId(...)).thenReturn(produtoComEstoque)`.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
@Test
void deveReservarQuandoHaEstoqueSuficiente() {
    Produto produto = new Produto("p1", 10);
    when(repositorio.buscarPorId("p1")).thenReturn(produto);

    boolean resultado = servico.reservar("p1", 3);

    assertTrue(resultado);
}
```
</details>

### Passo 3 — Reserva com estoque insuficiente

Configure o mock pra devolver um produto com apenas 2 unidades, e escreva um teste que tenta reservar 5 — deve retornar `false`.

<details>
<summary>💡 Dica</summary>

É o mesmo padrão do passo anterior, só que com um produto configurado com menos estoque do que o pedido.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
@Test
void naoDeveReservarQuandoEstoqueInsuficiente() {
    Produto produto = new Produto("p1", 2);
    when(repositorio.buscarPorId("p1")).thenReturn(produto);

    boolean resultado = servico.reservar("p1", 5);

    assertFalse(resultado);
}
```
</details>

### Passo 4 — Produto que não existe

Configure o mock pra devolver `null` (produto não encontrado), e verifique que o serviço lança `ProdutoNaoEncontradoException`.

<details>
<summary>💡 Dica</summary>

Combine `when(...).thenReturn(null)` com `assertThrows`, como visto na Aula 01 — os padrões se somam entre aulas.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
@Test
void deveLancarExcecaoQuandoProdutoNaoExiste() {
    when(repositorio.buscarPorId("p404")).thenReturn(null);

    assertThrows(
        ProdutoNaoEncontradoException.class,
        () -> servico.reservar("p404", 1)
    );
}
```
</details>

### Passo 5 — Confirmando que salvou

Escreva um teste que confirma, usando `verify`, que `repositorio.salvar(produto)` foi chamado depois de uma reserva bem-sucedida.

<details>
<summary>💡 Dica</summary>

Reveja o exemplo 4 da demonstração: `verify(mock).metodo(argumento)` confirma a interação, não só o retorno.
</details>

<details>
<summary>📖 Gabarito</summary>

```java
@Test
void deveSalvarProdutoApenasQuandoReservaBemSucedida() {
    Produto produto = new Produto("p1", 10);
    when(repositorio.buscarPorId("p1")).thenReturn(produto);

    servico.reservar("p1", 3);

    verify(repositorio).salvar(produto);
}
```
</details>

### Passo 6 — Rodar tudo e conferir

Rode a classe `ServicoDeEstoqueTest` completa e confirme que os 4 testes passam, sem depender de nenhum banco de dados real.

<details>
<summary>💡 Dica</summary>

Se algum teste falhar, confira se o mock foi configurado (`when`) antes de ser usado — mocks não configurados devolvem valores vazios, não erros.
</details>

<details>
<summary>📖 Saída esperada</summary>

```
$ mvn test

[INFO] Running ServicoDeEstoqueTest
✅ deveReservarQuandoHaEstoqueSuficiente
✅ naoDeveReservarQuandoEstoqueInsuficiente
✅ deveLancarExcecaoQuandoProdutoNaoExiste
✅ deveSalvarProdutoApenasQuandoReservaBemSucedida
[INFO] Tests run: 4, Failures: 0
```
</details>

---

## 🏆 Desafio

> Versão interativa com cronômetro e checklist: [`04_desafio.html`](./04_desafio.html)
> ⚠️ Este desafio **não tem gabarito** — é para você aplicar sozinho o que praticou.

### Missão: teste o `ServicoDeAutenticacao` sozinho

Use o que você praticou com `ServicoDePedido` (demonstração) e `ServicoDeEstoque` (prática guiada) para testar um serviço de login, mockando suas duas dependências.

**Classe sob teste:** `ServicoDeAutenticacao` · depende de `RepositorioUsuario` (método `buscarPorUsername(String username)`) e `CodificadorDeSenha` (método `verificar(String senhaDigitada, String hashArmazenado)`) · método `autenticar(String username, String senha)` retorna `boolean` · lança `UsuarioNaoEncontradoException` se o usuário não existir.

**Tempo sugerido:** 45 minutos.

### Checklist de requisitos

- [ ] **Estrutura inicial** — Criar `ServicoDeAutenticacaoTest` com `@Mock` para as duas dependências e `@InjectMocks` para o serviço.
- [ ] **Usuário não encontrado** — Configurar o repositório pra retornar `null` e verificar que `autenticar()` lança `UsuarioNaoEncontradoException`.
- [ ] **Senha correta** — Configurar o codificador pra retornar `true` e verificar que `autenticar()` retorna `true`.
- [ ] **Senha incorreta** — Configurar o codificador pra retornar `false` e verificar que `autenticar()` retorna `false`.
- [ ] **verify da senha** — Confirmar, com `verify`, que o codificador foi chamado com a senha digitada e o hash armazenado corretos.
- [ ] **verify(never())** — Confirmar que o codificador NUNCA é chamado quando o usuário não existe.
- [ ] **ArgumentCaptor** — Capturar o username passado ao repositório e confirmar que é exatamente o valor digitado, sem espaços extras.
- [ ] **Suíte 100% verde** — Rodar todos os testes da classe e confirmar que todos passam.

### Critérios de entrega

- Usa `@Mock` e `@InjectMocks` corretamente, sem instanciar nenhuma dependência real.
- Usa `when/thenReturn` para pelo menos 2 cenários diferentes (sucesso e falha).
- Usa `verify` — incluindo pelo menos um `never()` — para confirmar interações, não só retornos.
- A suíte roda 100% verde, sem alterar a classe `ServicoDeAutenticacao`.

Depois de concluir, seu professor vai registrar a avaliação na página de **Feedback** (`05_feedback.html`).

---

*Aula 04 · Testes Java · @karizeviecelli · 2026*
