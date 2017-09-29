# Recurso de carregamento: onload e onerror

O navegador permite rastrear o carregamento de recursos externos - scripts, iframes, imagens e assim por diante.

Há dois eventos para isso:

- `onload` - carga bem sucedida,
- `onerror` - ocorreu um erro.

## Carregando um script

Digamos que precisamos chamar uma função que reside em um script externo.

Podemos carregá-lo dinamicamente, assim:

`` `js
let script = document.createElement ('script');
script.src = "my.js";

document.head.append (script);
`` `

... Mas como executar a função que é declarada dentro desse script? Precisamos esperar até o script carregar, e só então podemos chamá-lo.

### script.onload

O principal ajudante é o evento `load`. Ele dispara depois que o script foi carregado e executado.

Por exemplo:

`` `js não são confiáveis
let script = document.createElement ('script');

// pode carregar qualquer script, de qualquer domínio
Script.asser = "Http: //cdnjas.cloudflare.com/aazax/libs/loadshjs/4.3.0/loadsh .js"
document.head.append (script);

*! *
script.onload = function () {
// o script cria uma função auxiliar "_"
alerta(_); // a função está disponível
};
* /! *
`` `

Então, no `onload` podemos usar variáveis ​​de script, executar funções etc.

... E e se o carregamento falhar? Por exemplo, não há tal script (erro 404) ou o servidor ou o servidor está desativado (não disponível).

### script.onerror

Os erros que ocorrem durante o carregamento (mas não a execução) do script podem ser rastreados no evento `error`.

Por exemplo, vamos solicitar um script que não exista:

`` `js run
let script = document.createElement ('script');
script.src = "https://example.com/404.js"; // nenhum script desse tipo
document.head.append (script);

*! *
script.onerror = function () {
alerta ("Erro ao carregar" + this.src); // Erro ao carregar https://example.com/404.js
};
* /! *
`` `

Por favor, note que não podemos obter detalhes de erro aqui. Não sabemos se foi erro 404 ou 500 ou qualquer outra coisa. Apenas o carregamento falhou.

## Outros recursos

Os eventos `load` e` error` também funcionam para outros recursos. No entanto, pode haver pequenas diferenças.

Por exemplo:

`<img>`, `<link>` (folhas de estilos externas)
: Os eventos `load` e` error` funcionam como esperado.

`<iframe>`
: Somente evento `load` quando o carregamento do iframe foi concluído. Isso desencadeia tanto a carga bem-sucedida quanto o caso de um erro. Isso é por razões históricas.

## Resumo

Imagens `<img>`, estilos externos, scripts e outros recursos fornecem eventos `load` e` error` para rastrear seu carregamento:

- 'load' dispara em uma carga bem sucedida,
- `error` desencadeia uma carga com falha.

A única exceção é `<iframe>`: por motivos históricos, ele sempre aciona "load", para qualquer conclusão da carga, mesmo que a página não seja encontrada.

O evento `readystatechange` também funciona para recursos, mas raramente é usado, porque os eventos 'load / error` são mais simples.
