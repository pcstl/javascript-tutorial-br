Uma janela modal pode ser implementada usando um meio-transparente `<div id =" cover-div ">` que cobre toda a janela, assim:

`` `css
# cover-div {
posição: fixo;
topo: 0;
esquerda: 0;
índice z: 9000;
largura: 100%;
altura: 100%;
cor de fundo: cinza;
opacidade: 0,3;
}
`` `

Porque o `<div>` abrange tudo, ele obtém todos os cliques, e não a página abaixo.

Também podemos evitar o deslocamento da página definindo `body.style.overflowY = 'hidden'`.

O formulário não deve estar no `<div>`, mas ao lado dele, porque não queremos ter "opacidade".
