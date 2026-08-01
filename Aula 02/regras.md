# Regras do Projeto — Curso de Testes Automatizados em Java

> `@karizeviecelli · 2026`

Este arquivo reúne as regras que devem ser seguidas na geração e revisão de todo o material do curso (slides, demonstrações, práticas, desafios, feedback e demais documentos).

## Regras

1. **Nunca usar referências que não sejam verificáveis.**
   Toda fonte, citação, link, dado técnico ou afirmação atribuída a terceiros deve poder ser conferida (documentação oficial, especificação, repositório público, artigo com autoria clara, etc.). Não usar fontes inventadas, vagas ("estudos mostram...") ou impossíveis de rastrear.

2. **Os links entre páginas devem ser válidos e apontar para a próxima página dentro da mesma pasta.**
   Toda navegação (botões "voltar", "avançar", CTAs como "Ir para a Demonstração →") deve usar caminho relativo simples (ex.: `02_demo.html`), assumindo que os arquivos da aula ficam juntos na mesma pasta. Nunca usar links quebrados, absolutos, ou que apontem para arquivos fora da pasta da aula.

3. **Toda página de uma aula precisa ter navegação nos dois sentidos (voltar E avançar), exceto a primeira e a última.**
   A primeira página (Exposição) só precisa de link para frente; a última (Feedback) só precisa de link para trás. Todas as páginas do meio (Demonstração, Prática, Desafio) precisam dos dois links visíveis. Exceção aceitável: um link de avanço pode ficar condicionado ao cumprimento de uma tarefa (ex.: Desafio → Feedback só libera com o checklist completo) — isso é intencional, não é bug.

4. **Bug conhecido do tokenizer de destaque de sintaxe (`realcar`): identificadores que começam com `$` travam o navegador em loop infinito.**
   A função usa `/[a-zA-Z_$]/` para detectar o início de um identificador, mas usava `/\w/` (que não inclui `$`) para avançar dentro dele — se a primeira letra for `$`, o índice nunca avança e o script trava a aba. Qualquer código de exemplo/gabarito com linhas de terminal (`$ mvn test`, `$ git commit`, etc.) aciona esse bug. **Sempre usar `/[a-zA-Z0-9_$]/` (mesmo conjunto de caracteres) tanto para detectar o início quanto para avançar dentro do identificador.** Antes de entregar qualquer arquivo com essa função, testar o carregamento com um script Node + jsdom (`runScripts: "dangerously"`) com timeout, simulando cliques nos elementos interativos, para garantir que nada trava.

---

*Este arquivo pode ser atualizado a qualquer momento — basta pedir para adicionar, remover ou ajustar uma regra.*
