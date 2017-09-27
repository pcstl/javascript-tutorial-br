# Ambiente do navegador, especificações

O idioma do JavaScript foi criado inicialmente para navegadores da web. Desde então, evoluiu e tornou-se um idioma com muitos usos e plataformas.

Uma plataforma pode ser um navegador ou um servidor web ou uma máquina de lavar ou outro * host *. Cada um deles fornece funcionalidades específicas da plataforma. A especificação de JavaScript chama aquele * ambiente de host *.

Um ambiente de host fornece objetos e funções específicos da plataforma adicionalmente ao núcleo do idioma. Os navegadores da Web dão meios para controlar páginas da web. Node.JS fornece recursos do lado do servidor, e assim por diante.

[cortar]

Aqui está uma visão panorâmica do que temos quando o JavaScript é executado em um navegador:

!] [] (windowObjects.png)

Existe um objeto "raiz" chamado `janela '. Tem duas funções:

1. Primeiro, é um objeto global para código JavaScript, conforme descrito no capítulo <info: global-object>.
2. Em segundo lugar, representa a "janela do navegador" e fornece métodos para controlá-la.

Por exemplo, aqui o usamos como um objeto global:

`` `js run
função sayHi () {

}

// funções globais estão acessíveis como propriedades da janela
window.sayHi ();
`` `

E aqui nós usamos isso como uma janela do navegador, para ver a altura da janela:

`` `js run
alerta (window.innerHeight); // altura interna da janela
`` `

Há mais métodos e propriedades específicos da janela, nós os abordaremos mais tarde.

## Document Object Model (DOM)

O objeto `document` dá acesso ao conteúdo da página. Podemos alterar ou criar qualquer coisa na página usando isso.

Por exemplo:
`` `js run
// altera a cor de fundo para o vermelho
document.body.style.background = 'red';

// alterá-lo depois de 1 segundo
setTimeout (() => document.body.style.background = '', 1000);
`` `

Aqui, usamos `document.body.style`, mas há muito, muito mais. Propriedades e métodos são descritos na especificação. Por acaso, existem dois grupos de trabalho que o desenvolvem:

1. [W3C] (https://en.wikipedia.org/wiki/World_Wide_Web_Consortium) - a documentação está em <https://www.w3.org/TR/dom>.
2. [WhatWG] (https://en.wikipedia.org/wiki/WHATWG), publicando em <https://dom.spec.whatwg.org>.

Como acontece, os dois grupos nem sempre concordam, então temos 2 conjuntos de padrões. Mas eles estão no contato apertado e, eventualmente, as coisas se fundem. Portanto, a documentação que você pode encontrar nos recursos fornecidos é muito similar, há como 99% de correspondência. Há diferenças muito menores, você provavelmente não vai notar.

Pessoalmente, acho <https://dom.spec.whatwg.org> mais agradável de usar.

No passado antigo, não havia nenhum padrão - cada navegador implementava o que desejava. Então, diferentes navegadores tinham diferentes métodos e propriedades para o mesmo, e os desenvolvedores precisavam escrever um código diferente para cada um deles. Épocas obscenas e confusas.

Mesmo agora, às vezes podemos encontrar um código antigo que usa propriedades específicas do navegador e funciona em torno de incompatibilidades. Mas neste tutorial usaremos coisas modernas: não há necessidade de aprender coisas antigas até que você realmente precise delas (as chances são altas, você não).

Em seguida, o padrão DOM apareceu, na tentativa de levar todos a um acordo. A primeira versão foi "DOM Level 1", então foi estendida pelo DOM Level 2, então DOM Level 3, e agora é DOM Level 4. Pessoas do grupo WhatWG se cansaram de versão e estão chamando isso de "DOM", sem uma número. Então, nós faremos.

`` `smart header =" DOM ​​não é apenas para navegadores "
A especificação DOM explica a estrutura de um documento e fornece objetos para manipulá-lo. Existem instrumentos que não o utilizam.

Por exemplo, ferramentas do lado do servidor que baixam páginas HTML e processam-nas. No entanto, eles podem suportar apenas uma parte da especificação DOM.
`` `

`` `cabeçalho inteligente =" CSSOM para estilo "
As regras de CSS e as folhas de estilo são estruturadas não como HTML. Portanto, há uma especificação separada [CSSOM] (https://www.w3.org/TR/cssom-1/) que explica como eles são representados como objetos, como lê-los e escrevê-los.

O CSSOM é usado em conjunto com o DOM quando modificamos as regras de estilo para o documento. Na prática, porém, o CSSOM raramente é necessário, porque geralmente as regras CSS são estáticas. Nós raramente precisamos adicionar / remover regras CSS do JavaScript, então não vamos cobri-lo agora.
`` `

## BOM (parte da especificação HTML)

O Modelo de Objeto do Navegador (BOM) são objetos adicionais fornecidos pelo navegador (ambiente do host) para trabalhar com tudo, exceto o documento.

Por exemplo:

- O objeto [navigator] (mdn: api / Window / navigator) fornece informações básicas sobre o navegador e o sistema operacional. Existem muitas propriedades, mas duas mais conhecidas são: `navigator.userAgent` - sobre o navegador atual e` navigator.platform` - sobre a plataforma (pode ajudar a diferir entre Windows / Linux / Mac etc).
- O objeto [localização] (mdn: api / Window / location) permite ler o URL atual e redirecionar o navegador para um novo.

Veja como podemos usar o objeto `location`:

`` `js run
alerta (location.href); // mostra URL atual
se (confirmar ("Ir para wikipedia?")) {
location.href = 'https://wikipedia.org'; // redirecionar o navegador para outro URL
}
`` `

Funções `alert / confirm / prompt` também são parte da lista técnica: eles não estão diretamente relacionados ao documento, mas representam métodos de navegador puro para se comunicar com o usuário.


`` `cabeçalho inteligente =" especificação HTML "
BOM é a parte da [especificação HTML] geral (https://html.spec.whatwg.org).

Sim, você ouviu direito. A especificação HTML em <https://html.spec.whatwg.org> não é apenas sobre a "linguagem HTML" (tags, atributos), mas também cobre um monte de objetos, métodos e extensões de DOM específicas do navegador. Isso é "HTML em termos gerais".
`` `

## Resumo

Falando sobre os padrões, temos:

Especificação DOM
: Descreve a estrutura do documento, manipulações e eventos, veja <https://dom.spec.whatwg.org>.

Especificação CSSOM
: Descreve folhas de estilo e regras de estilo, manipulações com elas e sua vinculação a documentos, veja <https://www.w3.org/TR/cssom-1/>.

Especificação HTML
: Descreve o idioma HTML (tags etc) e também BOM (modelo de objeto do navegador) - várias funções do navegador: `setTimeout`,` alert`, `location` e assim por diante, veja <https://html.spec.whatwg.org >. É preciso a especificação DOM e estende-a com muitas propriedades e métodos adicionais.

Agora vamos aprender a aprender DOM, porque o documento desempenha o papel central na UI, e trabalhar com ele é a parte mais complexa.

Por favor, note os links acima, porque há tantas coisas para aprender, é impossível cobrir e lembrar de tudo.

Quando você gostaria de ler sobre uma propriedade ou um método - o manual da Mozilla em <https://developer.mozilla.org/en-US/search> é bom, mas a leitura das especificações correspondentes pode ser melhor: mais complexo e mais longo para ler, mas tornará seu conhecimento fundamental som e completo.
