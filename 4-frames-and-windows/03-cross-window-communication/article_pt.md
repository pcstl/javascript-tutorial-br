# Comunicação de janela cruzada

A política "Mesmo origem" (mesmo site) limita o acesso de janelas e quadros entre si.

A idéia é que, se tivermos duas janelas abertas: uma de `john-smith.com`, e outra é` gmail.com`, então não queremos um script de `john-smith.com` para ler nosso enviar.

[cortar]

## mesma origem [# mesma origem]

Dois URLs têm a mesma origem se tiverem o mesmo protocolo, domínio e porta.

Esses URLs compartilham a mesma origem:

- `http://site.com`
- `http://site.com/`
- `http: // site.com / my / page.html`

Estes não:

- <code> http: // <b> www. </ b> site.com </ code> (outro domínio: `www.` importa)
- <code> http: // <b> site.org </ b> </ code> (outro domínio: `.org` importa)
- <code> <b> https: // </ b> site.com </ code> (outro protocolo: `https`)
- <code> http://site.com: <b> 8080 </ b> </ code> (outra porta: `8080`)

Se tivermos uma referência a outra janela (um popup ou iframe), e essa janela vem da mesma origem, então podemos fazer tudo com ela.

Se vier de outra origem, então só podemos mudar sua localização. Por favor, note: não * leia * a localização, mas * modifique *, redirecione-o para outro local. Isso é seguro, porque o URL pode conter parâmetros sensíveis, portanto, lê-lo de outra origem é proibido, mas a mudança não é.

Além disso, tais janelas do Windows podem trocar mensagens. Logo sobre isso mais tarde.

`` `'ar cabeçalho =" Exclusão: subdomínios podem ser de mesma origem "

Há uma exclusão importante na mesma política de origem.

Se o Windows compartilhar o mesmo domínio de segundo nível, por exemplo `john.site.com`,` peter.site.com` e `site.com`, podemos usar o JavaScript para atribuir a` document.domain` o seu segundo- domínio de domínio `site.com`. Então estas janelas são tratadas como tendo a mesma origem.

Em outras palavras, todos esses documentos (incluindo o de `site.com`) devem ter o código:

`` `js
document.domain = 'site.com';
```

Então eles podem interagir sem limitações.

Isso só é possível para páginas com o mesmo domínio de segundo nível.
````

## Acessando um conteúdo do iframe

Um `<iframe>` é uma besta de duas faces. De um lado, é uma etiqueta, como "<script>` ou `<img>`. Do outro lado, é uma janela na janela.

A janela incorporada possui objetos separados `` documento` e `janela`.

Podemos acessá-los como o uso das propriedades:

- `iframe.contentWindow` é uma referência à janela dentro do` <iframe> `.
- `iframe.contentDocument` é uma referência ao documento dentro do` <iframe> `.

Quando acessamos uma janela incorporada, o navegador verifica se o iframe tem a mesma origem. Se não for assim, o acesso será negado (com exclusões acima mencionadas).

Por exemplo, aqui está um `<iframe>` de outra origem:

`` `html run
<iframe src = "https://example.com" id = "iframe"> </ iframe>

<script>
iframe.onload = function () {
// podemos obter a referência para a janela interna
Deixe iframeWindow = iframe.contentWindow;

experimentar {
// ... mas não para o documento dentro dele
let doc = iframe.contentDocument;
} catch (e) {
alerta (e); // Erro de segurança (outra origem)
}

// também não podemos ler o URL da página nele
experimentar {
alerta (iframe.contentWindow.location);
} catch (e) {
alerta (e); // Erro de segurança
}

// ... mas podemos mudá-lo (e assim carregar algo mais no iframe)!
iframe.contentWindow.location = '/'; // trabalho

iframe.onload = null; // limpe o manipulador, para executar este código apenas uma vez
};
</ script>
```

O código acima mostra erros para qualquer operação, exceto:

- Obtendo a referência para a janela interna `iframe.contentWindow`
- Alterar a sua localização.

`` `cabeçalho inteligente =" `iframe.onload` vs` iframe.contentWindow.onload` "
O evento `iframe.onload` é realmente o mesmo que` iframe.contentWindow.onload`. Ele dispara quando a janela incorporada é totalmente carregada com todos os recursos.

... Mas `iframe.onload` está sempre disponível, enquanto` iframe.contentWindow.onload` precisa da mesma origem.
```

E agora um exemplo com a mesma origem. Podemos fazer qualquer coisa com a janela incorporada:

`` `html run
<iframe src = "/" id = "iframe"> </ iframe>

<script>
iframe.onload = function () {
// faça apenas qualquer coisa
iframe.contentDocument.body.prepend ("Olá, mundo!");
};
</ script>
```

### Aguarde até que o iframe carregue

Quando um iframe é criado, ele imediatamente possui um documento. Mas esse documento é diferente do que finalmente carrega nele!

Aqui, olhe:


`` `html run
<iframe src = "/" id = "iframe"> </ iframe>

<script>
deixe oldDoc = iframe.contentDocument;
iframe.onload = function () {
deixe newDoc = iframe.contentDocument;
*!*
// o documento carregado não é o mesmo que inicial!
alerta (oldDoc == newDoc); // falso
*/!*
};
</ script>
```

Essa é realmente uma armadilha bem conhecida para desenvolvedores novatos. Não devemos trabalhar com o documento imediatamente, porque esse é o * documento incorreto *. Se configurarmos qualquer manipulador de eventos, serão ignorados.

... Mas o evento 'onload` desencadeia quando todo o iframe com todos os recursos é carregado. E se quisermos agir mais cedo, no `DOMContentLoaded` do documento incorporado?

Isso não é possível se o iframe vier de outra origem. Mas para a mesma origem, podemos tentar pegar o momento em que um novo documento aparece e, em seguida, configurar os manipuladores necessários, como este:

`` `html run
<iframe src = "/" id = "iframe"> </ iframe>

<script>
deixe oldDoc = iframe.contentDocument;

// cada 100 ms verifique se o documento é o novo
let timer = setInterval (() => {
se (iframe.contentDocument == oldDoc) retornar;

// novo documento, vamos configurar manipuladores
iframe.contentDocument.addEventListener ('DOMContentLoaded', () => {
iframe.contentDocument.body.prepend ("Olá, mundo!");
});

clearInterval (timer); // cancelar setInterval, não precisa mais disso
}, 100);
</ script>
```

Deixe-me saber em comentários se você conhece uma solução melhor aqui.

## window.frames

Uma maneira alternativa de obter um objeto de janela para `<iframe>` - é obtê-lo da coleção nomeada `window.frames`:

- Por número: `window.frames [0]` - o objeto de janela para o primeiro quadro no documento.
- Por nome: `window.frames.iframeName` - o objeto da janela para o quadro com` name = "iframeName" `.

Por exemplo:

`` `html run
<iframe src = "/" style = "height: 80px" name = "win" id = "iframe"> </ iframe>

<script>
alerta (iframe.contentWindow == frames [0]); // verdade
alerta (iframe.contentWindow == frames.win); // verdade
</ script>
```

Um iframe pode ter outros iframes dentro. Os objetos `window` correspondentes formam uma hierarquia.

Os links de navegação são:

- `window.frames` - a coleção de janelas" infantis "(para quadros aninhados).
- `window.parent` - a referência para a janela" pai "(exterior).
- `window.top` - a referência para a janela principal mais alta.

Por exemplo:

`` `js run
window.frames [0] .parent === janela; // verdade
```

Podemos usar a propriedade `top` para verificar se o documento atual está aberto dentro de um quadro ou não:

`` `js run
se (janela == topo) {// janela atual == window.top?
alerta ('O script está na janela superior, não em um quadro');
} outro {
alerta ('O script é executado em um quadro!');
}
```

## O atributo sandbox

O atributo `sandbox` permite proibir determinadas ações dentro de um` <iframe> `, para executar um código não confiável. É "sandboxes" o iframe, tratando-o como vindo de outra origem e / ou aplicando outras limitações.

Por padrão, para `<iframe sandbox src =" ... ">` o "conjunto padrão" de restrições é aplicado ao iframe. Mas podemos fornecer uma lista separada por espaço de limitações "excluídas" como um valor do atributo, como este: `<iframe sandbox =" allow-forms allow-popups ">`. As limitações listadas não são aplicadas.

Em outras palavras, um atributo vazio de "sandbox" atribui as limitações mais estritas possíveis, mas podemos colocar uma lista delimitada por espaço daqueles que queremos levantar.

Aqui está uma lista de limitações:

`allow-same-origin``s
: Por padrão `" sandbox "` força a política de "origem diferente" para o iframe. Em outras palavras, faz o navegador tratar o `iframe` como vindo de outra origem, mesmo que o` src` aponte para o mesmo site. Com todas as restrições implícitas para scripts. Esta opção remove esse recurso.

`allow-top-navigation`
: Permite que o `iframe` altere` parent.location`.

`allow-forms`
: Permite enviar formulários do `iframe`.

`allow-scripts`
: Permite executar scripts do `iframe`.

`allow-popups`
: Permite 'window.open` popups do `iframe`

Veja [o manual] (mdn: / HTML / Elemento / iframe) para mais.

O exemplo a seguir demonstra um iframe em caixa de areia com o conjunto de restrições padrão: `<iframe sandbox src =" ... ">`. Tem algum JavaScript e um formulário.

Por favor note que nada funciona. Então o conjunto padrão é realmente áspero:

[codetabs src = "sandbox" height = 140]


`` `esperto
A finalidade do atributo `" sandbox "` é apenas para * adicionar mais * restrições. Não pode removê-los. Em particular, não pode relaxar as restrições da mesma origem se o iframe for proveniente de outra origem.
```

## Mensagens de janela cruzada

A interface `postMessage` permite que o Windows fale uns com os outros, independentemente de sua origem.

Tem duas partes.

### postar mensagem

A janela que quer enviar uma mensagem chama o método [postMessage] (mdn: api / Window.postMessage) da janela de recebimento. Em outras palavras, se queremos enviar a mensagem para `win`, devemos chamar` win.postMessage (data, targetOrigin) `.

Argumentos:

`data`
: Os dados a enviar. Pode ser qualquer objeto, os dados são clonados usando o "algoritmo de clonagem estruturado". O IE suporta apenas strings, então devemos "JSON.stringify" objetos complexos para suportar esse navegador.

`targetOrigin`
: Especifica a origem para a janela de destino, de modo que apenas uma janela da origem fornecida receberá a mensagem.

O `targetOrigin` é uma medida de segurança. Lembre-se, se a janela de destino vem de outra origem, não podemos ler isso é `localização`. Portanto, não podemos ter certeza de qual site está aberto na janela pretendida agora: o usuário pode navegar.

Especificar `targetOrigin` garante que a janela somente recebe os dados se ainda estiver nesse site. Bom quando os dados são sensíveis.

Por exemplo, aqui `win` só receberá a mensagem se tiver um documento da origem` http: // example.com`:

`` `html não embelezam
<iframe src = "http://example.com" name = "example">

<script>
deixe win = window.frames.example;

win.postMessage ("mensagem", "http://example.com");
</ script>
```

Se não queremos esse cheque, podemos configurar `targetOrigin` para` * `.

`` `html não embelezam
<iframe src = "http://example.com" name = "example">

<script>
deixe win = window.frames.example;

*!*
win.postMessage ("mensagem", "*");
*/!*
</ script>
```


### onmessage

Para receber uma mensagem, a janela de destino deve ter um manipulador no evento `message`. Isso dispara quando o `postMessage` é chamado (e a verificação` targetOrigin` é bem-sucedida).

O objeto de evento tem propriedades especiais:

`data`
: Os dados de `postMessage`.

`origem '
: Origem do remetente, por exemplo `http: // javascript.info`.

`fonte '
: A referência à janela do remetente. Nós podemos imediatamente "postMessage" de volta se quisermos.

Para atribuir esse manipulador, devemos usar `addEventListener`, uma sintaxe curta` window.onmessage` não funciona.

Aqui está um exemplo:

`` `js
window.addEventListener ("mensagem", função (evento) {
se (event.origin! = 'http://javascript.info') {
// algo de um domínio desconhecido, vamos ignorá-lo
Retorna;
}

alerta ("received:" + event.data);
});
```

O exemplo completo:

[codetabs src = "postmessage" height = 120]

`` `cabeçalho inteligente =" Não há atraso "
Não há sem demora entre o `postMessage` eo evento` message`. Isso acontece de forma síncrona, ainda mais rápido do que `setTimeout (..., 0)`.
```

## Resumo

Para chamar métodos e acessar o conteúdo de outra janela, primeiro devemos ter uma referência a ele.

Para pop-ups, temos duas propriedades:
- `window.open` - abre uma nova janela e retorna uma referência a ela,
- `window.opener` - uma referência à janela do abridor a partir de uma janela emergente

Para iframes, podemos acessar janelas pai / filho usando:
- `window.frames` - uma coleção de objetos de janela aninhada,
- `window.parent`,` window.top` são as referências às janelas principais e principais,
- `iframe.contentWindow` é a janela dentro de uma tag` <iframe> `.

Se o Windows compartilhar a mesma origem (host, porta, protocolo), o Windows pode fazer o que quiserem um com o outro.

Caso contrário, apenas as ações possíveis são:
- Alterar a localização de outra janela (acesso somente para gravação).
- Poste uma mensagem para ele.

As exclusões são:
- Windows que compartilha o mesmo domínio de segundo nível: `a.site.com` e` b.site.com`. Em seguida, configurar `document.domain = 'site.com' em ambos os coloca no estado da" mesma origem ".
- Se um iframe tem um atributo `sandbox`, ele é colocado vigorosamente no estado de" origem diferente ", a menos que o` allow-same-origin`` seja especificado no valor do atributo. Isso pode ser usado para executar código não confiável em iframes do mesmo site.

A interface `postMessage` permite que duas janelas conversem com verificações de segurança:

1. O remetente chama `targetWin.postMessage (data, targetOrigin)`.
2. Se `targetOrigin` não for` '*' `, o navegador verificará se a janela` targetWin` possui o URL do site 'targetWin`.
3. Se for assim, o "targetWin" desencadeia o evento `message` com propriedades especiais:
- `origem` - a origem da janela do remetente (como` http: // my.site.com`)
- `source` - a referência à janela do remetente.
- `data` - os dados, qualquer objeto em todos os lugares, exceto o IE que suporta apenas strings.

Devemos usar `addEventListener` para configurar o manipulador para este evento dentro da janela de destino.
