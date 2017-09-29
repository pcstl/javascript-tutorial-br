O núcleo da solução é uma função que adiciona mais datas à página (ou carrega mais coisas na vida real) enquanto estamos no final da página.

Podemos chamá-lo imediatamente e adicionar como um manipulador `window.onscroll`.

A questão mais importante é: "Como detectamos que a página está rolando para o fundo?"

Vamos usar as coordenadas relativas à janela.

O documento é representado (e contido) na tag `<html>`, que é `document.documentElement`.

Podemos obter coordenadas relativas à janela de todo o documento como `document.documentElement.getBoundingClientRect ()`. E a propriedade `bottom` será coordenada relativa à janela do final do documento.

Por exemplo, se a altura de todo o documento HTML for 2000px, então:

`` `js
// Quando estamos no topo da página
// window-relative top = 0
document.documentElement.getBoundingClientRect (). top = 0

// window-relative bottom = 2000
// o documento é longo, de modo que provavelmente está muito além do fundo da janela
document.documentElement.getBoundingClientRect (). bottom = 2000
`` `

Se rolarmos `500px` abaixo, então:

`` `js
// top do documento está acima da janela 500px
document.documentElement.getBoundingClientRect (). top = -500
// fundo do documento é 500px mais perto
document.documentElement.getBoundingClientRect (). bottom = 1500
`` `

Quando rolar até o final, assumindo que a altura da janela é `600px`:


`` `js
// top do documento está acima da janela 500px
document.documentElement.getBoundingClientRect (). top = -1400
// fundo do documento é 500px mais perto
document.documentElement.getBoundingClientRect (). bottom = 600
`` `

Por favor, note que a parte inferior não pode ser 0, porque nunca atinge a parte superior da janela. O limite mais baixo da coordenada inferior é a altura da janela, não podemos rolar mais.

E a altura da janela é `document.documentElement.clientHeight`.

Queremos que o documento não seja mais de 100px.

Então, aqui está a função:

`` `js
função preenchida () {
enquanto (verdadeiro) {
// documento inferior
Deixe windowRelativeBottom = document.documentElement.getBoundingClientRect (). bottom;

// se for maior que a altura da janela + 100px, então não estamos na página de volta
// (veja exemplos acima, fundo grande significa que precisamos rolar mais)
se (windowRelativeBottom> document.documentElement.clientHeight + 100) quebrar;

// caso contrário, vamos adicionar mais dados
document.body.insertAdjacentHTML ("beforeend", `<p> Data: $ {new Date ()} </ p>`);
}
}
`` `
