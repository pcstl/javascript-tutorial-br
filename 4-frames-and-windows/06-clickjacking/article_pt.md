# O ataque de clique

O ataque "clickjacking" permite que uma página doente clique em um "site da vítima" * em nome do visitante *.

Muitos sites foram hackeados desta forma, incluindo Twitter, Facebook, Paypal e outros sites. Todos estão consertados, é claro.

[cortar]

## A ideia

A ideia é muito simples.

Veja como o clickjacking foi feito com o Facebook:

1. Um visitante é atraído para a página malvada. Não importa como.
2. A página tem um link inofensivo sobre ele (como "ficar rico agora" ou "clicar aqui, muito engraçado" e assim por diante).
3. Sobre esse link, a página do mal posiciona um <iframe> `transparente com` src` do facebook.com, de tal forma que o botão "Curtir" está bem acima desse link. Geralmente isso é feito com `z-index`.
4. Ao clicar nesse link, o visitante pressiona esse botão.

## A demo

Veja como a página do mal parece. Para tornar as coisas claras, o `<iframe>` é meio transparente (em páginas malvadas reais é totalmente transparente):

`` `html run height = 120 não-embelezar
<style>
iframe {/ * iframe do site da vítima * /
largura: 400px;
altura: 100px;
posição: absoluta;
topo: 0; esquerda: -20px;
*!*
opacidade: 0,5; / * em opacidade real: 0 * /
*/!*
índice z: 1;
}
</ style>

<div> Clique para ficar rico agora: </ div>

<! - O URL do site da vítima ->
*!*
<iframe src = "facebook.html"> </ iframe>

<botão> Clique aqui! </ button>
*/!*

<div> ... E você é legal (eu sou um hacker legal na verdade)! </ div>
```

A demonstração completa do ataque:

[codetabs src = "clickjacking-visível" height = 160]

Aqui temos um meio-transparente `<iframe src =" facebook.html ">`, e no exemplo podemos vê-lo pairando sobre o botão. Um clique no botão realmente clica no iframe, mas isso não é visível para o usuário, porque o iframe é transparente.

Como resultado, se o visitante estiver autorizado no Facebook ("lembre-me" geralmente é ativado), então ele adiciona um "Curtir". No Twitter, seria um botão "Seguir".

Aqui está o mesmo exemplo, mas mais próximo da realidade, com `opacity: 0` para` <iframe> `:

[codetabs src = "clickjacking" height = 160]

Tudo o que precisamos atacar - é posicionar o `<iframe>` na página dobrável de tal forma que o botão esteja bem sobre o link. Isso geralmente é possível com CSS.

`` `cabeçalho inteligente =" Clickjacking é para cliques, não para teclado "
O ataque apenas afeta as ações do mouse.

Tecnicamente, se tivermos um campo de texto para piratear, podemos posicionar um iframe de tal forma que os campos de texto se sobrepõem. Então, quando um visitante tenta se concentrar na entrada que ele vê na página, ele realmente se concentra na entrada dentro do iframe.

Mas então há um problema. Tudo o que os tipos de visitantes serão escondidos, porque o iframe não está visível.

Então, isso parece muito estranho para o usuário, e ele vai parar.
```

## Defesa da velha escola (fraca)

O método de defesa mais antigo é o pedaço de JavaScript que proíbe a abertura da página em um quadro (o chamado "framebusting").

Como isso:

`` `js
se (topo! = janela) {
top.location = window.location;
}
```

Isto é: se a janela descobre que não está no topo, então ele automaticamente se torna o topo.

A partir de agora, isso não é confiável, porque existem muitas maneiras de invadir isso. Vamos cobrir alguns.

### Bloqueando navegação superior

Podemos bloquear a transição causada pela mudança de `top.location` no evento [onbeforeunload] (info: onload-domcontentloaded # window.onbeforeunload).

A página superior (que pertence ao hacker) define um manipulador para ele, e quando o `iframe` tenta mudar` top.location`, o visitante recebe uma mensagem perguntando se ele quer sair.

Como isso:
`` `js
window.onbeforeunload = function () {
window.onbeforeunload = null;
retorno "Quer sair sem aprender todos os segredos (ele-ele)?";
};
```

Na maioria dos casos, o visitante responderia negativamente, porque ele não conhece o iframe, tudo o que ele pode ver é a página de topo, não há motivo para sair. E assim o `top.location` não vai mudar!

Em ação:

[codetabs src = "top-location"]

### atributo Sandbox

Uma das coisas restritas pelo atributo `sandbox` é a navegação. Um iframe com caixa de areia pode não alterar `top.location`.

Então, podemos adicionar o iframe com `sandbox =" allow-scripts allow-forms "`. Isso relaxaria as restrições que permitem scripts e formulários. Mas não colocamos `allow-top-navigation` no valor para que a navegação ainda esteja proibida. E a mudança de `top.location` não funcionará.

Aqui está o código:

`` `html
<iframe *! * sandbox = "allow-scripts allow-forms" * /! * src = "facebook.html"> </ iframe>
```

Existem outras maneiras de contornar essa proteção simples também.

## X-Frame-Options

O cabeçalho do lado do servidor `X-Frame-Options` pode permitir ou proibir mostrar a página dentro de um quadro.

Ele deve ser enviado pelo servidor: o navegador ignorá-lo se encontrado nas tags `<meta>`. Então `<meta http-equiv =" X-Frame-Options "...>` não fará nada.

O cabeçalho pode ter 3 valores:


`DENY`
: Nunca mostre a página dentro de um iframe.

`SAMEORIGIN`
: Permite mostrar dentro de um iframe se o documento original vem da mesma origem.

`PERMITIR-FROM domain`
: Permite mostrar dentro de um iframe se o documento original é do domínio dado.

Por exemplo, o Twitter usa `X-Frame-Options: SAMEORIGIN`. Aqui está o resultado:

`` `html
<iframe src = "https://twitter.com"> </ iframe>
```

<iframe src = "https://twitter.com"> </ iframe>

Dependendo do navegador, `iframe` acima está vazio ou tem uma mensagem dizendo que" o navegador não pode mostrá-lo ".

## Mostrando com funcionalidade desabilitada

O cabeçalho `X-Frame-Options` protegido tem um efeito colateral. Outros sites não podem mostrar nossa página em um `iframe`, mesmo que tenham razões" legais "para fazê-lo.

Portanto, existem outras soluções. Por exemplo, podemos "cobrir" a página com um `<div>` com `height: 100%; width: 100%`, de modo que ele lida com todos os cliques. Esse `<div>` deve desaparecer se `window == top` ou descobrimos que não precisamos de proteção.

Como isso:

`` `html
<style>
#protector {
altura: 100%;
largura: 100%;
posição: absoluta;
esquerda: 0;
topo: 0;
z-index: 99999999;
}
</ style>

<div id = "protetor">
<a href="/" target="_blank"> Ir para o site </a>
</ div>

<script>
// haverá um erro se a janela superior for da origem diferente
//, mas está ok aqui
se (top.document.domain == document.domain) {
protector.remove ();
}
</ script>
```

A demo:

[codetabs src = "protetor"]

## Resumo

Clickjacking é uma maneira de "enganar" os usuários em um clique no site da vítima, sem sequer saber o que acontece. Isso é perigoso se houver ações importantes ativadas por clique.

Um hacker pode postar um link para sua página malvada em uma mensagem ou atrair visitantes para sua página por outros meios. Existem muitas variantes.

De um lado - o ataque não é "profundo": tudo que um hacker pode fazer é um clique. Mas, de outro lado, se o hacker sabe que, após o clique, outro controle aparece, então ele pode usar mensagens astutas para fazer com que o usuário também clique nela.

O ataque é bastante perigoso, porque quando criamos a UI, geralmente não pensamos que um hacker possa clicar em nome do visitante. Portanto, as vulnerabilidades podem ser encontradas em lugares totalmente inesperados.

- Recomenda-se usar `X-Frame-Options: SAMEORIGIN` em páginas que não devem ser mostradas dentro de iframes (ou apenas para todo o site).
- Use uma cobertura `<div>` se quisermos permitir que nossas páginas sejam mostradas em iframes e ainda permaneçam seguras.
