
# HTML / CSS
Primeiro vamos criar HTML / CSS.

Um menu é um componente gráfico autônomo na página, por isso é melhor colocá-lo em um único elemento DOM.

Uma lista de itens de menu pode ser apresentada como uma lista `ul / li`.

Aqui está a estrutura de exemplo:

`` `html
<div class = "menu">
<span class = "title"> Sweeties (clique em mim)! </ span>
<Ul>
<Li> Cake </ li>
<Li> Donut </ li>
<Li> Honey </ li>
</ Ul>
</ div>
`` `

Usamos `<span>` para o título, porque `<div>` tem uma 'exibição implícita': bloco 'nele, e ocupará 100% da largura horizontal.

Como isso:

`` `html autorun height = 50

`` `

Então, se configurarmos 'onclick` nele, então ele irá capturar cliques à direita do texto.

... mas `<span>` tem uma 'exibição implícita' inline`, então ocupa exatamente o espaço suficiente para caber todo o texto:

`` `html autorun height = 50

`` `

# Alternando o menu

Alternar o menu deve mudar a seta e mostrar / ocultar a lista do menu.

Todas essas alterações são perfeitamente tratadas pelo CSS. Em JavaScript, devemos rotular o estado atual do menu adicionando / removendo a classe `.open`.

Sem ele, o menu será fechado:

`` `css
.menu ul {
margem: 0;
estilo de lista: nenhum;
preenchimento-esquerda: 20px;
exibir: nenhum;
}

.menu .title :: before {
conteúdo: '▶';
font-size: 80%;
cor: verde;
}
`` `

... E com `.open` a seta muda e a lista aparece:

`` `css
.menu.open .title :: before {
conteúdo: '▼';
}

.menu.open ul {
exibição: bloco;
}
`` `
