importância: 5

---

# Pegar links no elemento

Faça todos os links dentro do elemento com `id =" contents "` pergunte ao usuário se ele realmente quer sair. E se ele não, então, não segue.

Como isso:

[iframe height = 100 border = 1 src = "solution"]

Detalhes:

- O HTML dentro do elemento pode ser carregado ou regenerado dinamicamente a qualquer momento, portanto, não podemos encontrar todos os links e colocar manipuladores sobre eles. Use a delegação do evento.
- O conteúdo pode ter tags aninhadas. Dentro de links também, como `<a href=".."> <i> ... </ i> </a>`.
