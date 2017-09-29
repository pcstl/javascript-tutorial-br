# Deslocamento

Os eventos de deslocamento permitem reagir em uma página ou rolagem de elementos. Há algumas coisas boas que podemos fazer aqui.

Por exemplo:
- Mostrar / ocultar controles adicionais ou informações, dependendo de onde no documento o usuário esteja.
- Carregue mais dados quando o usuário desloca-se até o final da página.

[cortar]

Aqui está uma pequena função para mostrar o rolo atual:

`` `js autorun
window.addEventListener ('scroll', function () {
document.getElementById ('showScroll'). innerHTML = pageYOffset + 'px';
});
`` `

`` `online
Em ação:

Navegação atual = <b id = "showScroll"> role a janela </ b>
`` `

O evento `scroll` funciona tanto na 'janela' quanto nos elementos roláveis.

## Evite a rolagem

Como podemos fazer algo insuperável? Não podemos impedir a rolagem usando `event.preventDefault ()` no ouvinte `onscroll`, porque ele dispara * após * o pergaminho já aconteceu.

Mas podemos evitar a rolagem por `event.preventDefault ()` em um evento que causa o pergaminho.

Por exemplo:
- evento `wheel` - um rolo da roda do mouse (uma ação do touchpad" rolagem "gera também).
- evento `keydown` para` key: pageUp` e `key: pageDown`.

Às vezes, isso pode ajudar. Mas há mais maneiras de rolar, por isso é bastante difícil lidar com todos eles. Portanto, é mais confiável usar o CSS para tornar algo insuperável, como propriedade de "transbordamento".

Aqui estão algumas tarefas que você pode resolver ou olhar para ver os aplicativos em `onscroll`.
