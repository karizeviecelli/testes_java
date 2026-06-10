# ☕ Testes em Java — Materiais Didáticos

> Curso completo de testes automatizados em Java para turmas de **Desenvolvimento de Sistemas**.
> 15 aulas · 4 horas cada · 60 horas totais

**Professora:** [@karizeviecelli](https://github.com/karizeviecelli)  
**Ano:** 2026  
**Nível:** Técnico em Informática / Desenvolvimento de Sistemas

---

## 📋 Sobre o Projeto

Este repositório contém os materiais didáticos interativos do curso de **Testes em Java**, cobrindo desde os fundamentos do JUnit 5 até testes avançados de arquitetura com ArchUnit.

Cada aula é composta por **5 páginas HTML independentes** que seguem uma progressão pedagógica do conteúdo teórico até a avaliação prática:

| Arquivo | Tipo | Descrição |
|---|---|---|
| `01_exposicao.html` | 📖 Exposição | Slides com navegação, timer e destaque de conceitos |
| `02_demo.html` | 💻 Demonstração | Editor com efeito typewriter, syntax highlight Java e 5 exemplos práticos |
| `03_pratica.html` | 🛠 Prática guiada | Checklist de passos com dicas, gabarito revelável e barra de progresso |
| `04_desafio.html` | 🏆 Desafio | Missão autônoma com cronômetro, requisitos e critérios de entrega |
| `05_feedback.html` | 📊 Feedback | Painel do professor com avaliação por estrelas e localStorage |

---

## 🗂 Estrutura de Pastas

```
📁 Testes-Java/
├── 📄 README.md
├── 📄 plano_testes_java.html        ← Plano de ensino navegável (publicar como GitHub Pages)
│
├── 📁 Aula 01/                      ← JUnit 5 & Primeiro Teste
│   ├── 01_exposicao.html
│   ├── 02_demo.html
│   ├── 03_pratica.html
│   ├── 04_desafio.html
│   └── 05_feedback.html
│
├── 📁 Aula 02/                      ← Assertions Avançadas & @ParameterizedTest
├── 📁 Aula 03/                      ← TDD — Red Green Refactor
├── 📁 Aula 04/                      ← Mockito — Mocks e Stubs
├── 📁 Aula 05/                      ← Spring Boot Test & MockMvc
├── 📁 Aula 06/                      ← JaCoCo — Cobertura de Código
├── 📁 Aula 07/                      ← Mutation Testing com PIT
├── 📁 Aula 08/                      ← JMH — Testes de Performance
├── 📁 Aula 09/                      ← RestAssured — API REST
├── 📁 Aula 10/                      ← Pact JVM — Testes de Contrato
├── 📁 Aula 11/                      ← Gatling — Testes de Carga
├── 📁 Aula 12/                      ← Segurança — OWASP & Java
├── 📁 Aula 13/                      ← ArchUnit — Testes de Arquitetura
├── 📁 Aula 14/                      ← Projeto Integrador — Desenvolvimento
└── 📁 Aula 15/                      ← Apresentação e Retrospectiva Final
```

---

## 🗺 Mapa do Curso

### 🟧 Bloco 1 — Fundamentos (Aulas 01–03)
| Aula | Tema | Ferramentas |
|------|------|-------------|
| 01 | JUnit 5 & Primeiro Teste | JUnit Jupiter, Maven Surefire, IntelliJ IDEA |
| 02 | Assertions Avançadas & @ParameterizedTest | @CsvSource, @ValueSource, assertThrows, assertAll |
| 03 | TDD — Red Green Refactor | FizzBuzz Kata, Bowling Kata, Conventional Commits |

### 🔵 Bloco 2 — Mocks & Integração (Aulas 04–05)
| Aula | Tema | Ferramentas |
|------|------|-------------|
| 04 | Mockito — Mocks e Stubs | Mockito 5, @Mock, @InjectMocks, ArgumentCaptor |
| 05 | Spring Boot Test & MockMvc | @WebMvcTest, @DataJpaTest, MockMvc, jsonPath |

### 🟢 Bloco 3 — Qualidade & Cobertura (Aulas 06–07)
| Aula | Tema | Ferramentas |
|------|------|-------------|
| 06 | JaCoCo — Cobertura de Código | jacoco-maven-plugin, relatório HTML, GitHub Actions |
| 07 | Mutation Testing com PIT | pitest-maven, pitest-junit5-plugin, mutation score |

### 🟣 Bloco 4 — Tipos Especializados (Aulas 08–13)
| Aula | Tema | Ferramentas |
|------|------|-------------|
| 08 | JMH — Testes de Performance | JMH 1.37, @Benchmark, @Param, BenchmarkMode |
| 09 | RestAssured — API REST | RestAssured 5, given/when/then, schema validation |
| 10 | Pact JVM — Testes de Contrato | Pact JVM, Pact Broker, consumer-driven contracts |
| 11 | Gatling — Testes de Carga | Gatling 3, ramp-up, SLA assertions, feeders CSV |
| 12 | Segurança — OWASP & Java | OWASP Dependency-Check, SpotBugs, Find Security Bugs |
| 13 | ArchUnit — Testes de Arquitetura | ArchUnit 1.3, layered architecture, naming rules |

### 🟠 Bloco 5 — Projeto Integrador (Aulas 14–15)
| Aula | Tema | Descrição |
|------|------|-----------|
| 14 | Projeto Integrador — Dev | API Spring Boot com todos os tipos de teste e CI verde |
| 15 | Apresentação Final | Defesa técnica, demo ao vivo, retrospectiva |

---

## 🚀 Como Usar

### Pré-requisitos

- Navegador moderno (Chrome, Firefox, Edge ou Safari)
- Nenhuma instalação ou servidor necessário — todos os arquivos são HTML estático

### Uso pelo Professor

**1. Clone ou baixe o repositório**

```bash
git clone https://github.com/karizeviecelli/testes-java.git
cd testes-java
```

**2. Abra o plano de ensino**

Abra o arquivo `plano_testes_java.html` diretamente no navegador para ter uma visão geral navegável de todas as 15 aulas.

**3. Conduza cada aula em sequência**

Para cada aula, abra os arquivos na ordem sugerida:

```
Aula 01/01_exposicao.html   → Apresentar os slides para a turma (projetor)
Aula 01/02_demo.html        → Demonstrar os 5 exemplos ao vivo
Aula 01/03_pratica.html     → Acompanhar enquanto os alunos praticam
Aula 01/04_desafio.html     → Alunos trabalham de forma autônoma
Aula 01/05_feedback.html    → Registrar avaliações individuais
```

**4. Registrar avaliações (Feedback)**

No arquivo `05_feedback.html`:
- Clique em **+** para adicionar um aluno
- Atribua estrelas (1–5)
- Marque os critérios alcançados
- Adicione observações
- Clique em **Salvar** — os dados ficam no `localStorage` do navegador

> ⚠️ Os dados de feedback ficam salvos no navegador. Use sempre o mesmo computador/perfil de navegador para não perder os registros.

### Uso pelo Aluno

**1. Receba o link ou pasta da aula**

O professor pode compartilhar a pasta da aula via Google Drive, pendrive ou GitHub Pages.

**2. Navegue pelos arquivos na ordem**

```
01 → Leia os slides e anote as dúvidas
02 → Clique em cada exemplo e observe a execução
03 → Siga os passos e revele o gabarito só após tentar
04 → Resolva o desafio no tempo indicado
```

**3. Acompanhe seu progresso**

- Na **Prática (03):** marque cada passo como concluído — a barra de progresso avança
- No **Desafio (04):** marque os requisitos cumpridos e inicie o cronômetro
- O botão "Feedback →" só ativa quando todos os requisitos estiverem marcados

---

## 🌐 Publicar como GitHub Pages

Para disponibilizar o plano de ensino online:

**1. Habilite o GitHub Pages no repositório**

```
Settings → Pages → Source: Deploy from a branch → Branch: main → / (root)
```

**2. Acesse pelo link gerado**

```
https://karizeviecelli.github.io/testes-java/plano_testes_java.html
```

**3. Compartilhe com os alunos**

O link pode ser usado diretamente no celular, tablet ou computador — sem instalação.

---

## 🎨 Personalizar

### Alterar nome/ano no rodapé

Todos os rodapés usam o padrão:
```
@karizeviecelli · 2026
```

Para trocar, faça busca e substituição em todos os arquivos `.html`:

```bash
# Linux / macOS
find . -name "*.html" -exec sed -i 's/@karizeviecelli/@@seunome/g' {} \;
find . -name "*.html" -exec sed -i 's/2026/2027/g' {} \;
```

### Adicionar alunos pré-cadastrados no Feedback

Edite o `05_feedback.html` de cada aula e adicione no `localStorage` via console do navegador:

```js
localStorage.setItem('feedback_java_aula01', JSON.stringify([
  { nome: "Ana Silva", estrelas: 0, crit: [false,false,false,false,false], obs: "", status: "Ativo" },
  { nome: "João Santos", estrelas: 0, crit: [false,false,false,false,false], obs: "", status: "Ativo" }
]));
```

---

## 📦 Tecnologias Utilizadas

Os materiais são **100% HTML, CSS e JavaScript vanilla** — sem frameworks, sem dependências externas de runtime, sem servidor necessário.

| Tecnologia | Uso |
|---|---|
| HTML5 | Estrutura das páginas |
| CSS3 | Layout, animações, temas por aula |
| JavaScript ES6 | Interatividade, typewriter effect, timers |
| `localStorage` | Persistência de feedback e progresso |
| Google Fonts | Tipografia (carregada via CDN) |

---

## 🤝 Contribuindo

Sugestões de melhoria, correções de conteúdo ou novas ferramentas são bem-vindas.

1. Faça um fork do repositório
2. Crie uma branch: `git checkout -b melhoria/aula-07-pitest`
3. Commit com Conventional Commits: `git commit -m "fix(aula07): corrigir exemplo de mutante sobrevivente"`
4. Abra um Pull Request

---

## 📄 Licença

Este material é de uso educacional livre.  
Permitido usar, adaptar e redistribuir com atribuição à autora.

---

<div align="center">

Feito com ☕ por **[@karizeviecelli](https://github.com/karizeviecelli)** · 2026

*"Código sem teste é código que alguém ainda não quebrou."*

</div>
