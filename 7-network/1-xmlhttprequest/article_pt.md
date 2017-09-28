# XMLHttpRequest e AJAX

`XMLHttpRequest` é um objeto de navegador embutido que permite fazer solicitações HTTP em JavaScript.

Apesar de ter a palavra "XML" em seu nome, ele pode operar em qualquer dado, não apenas em formato XML.

## Assincrona XMLHttpRequest

XMLHttpRequest tem dois modos de operação: síncrono e assíncrono.

Primeiro, vejamos a variante assíncrona, pois é usada na maioria dos casos.

O código abaixo carrega o URL no `/ article / xmlhttprequest / hello.txt` do servidor e mostra o conteúdo na tela:

`` `js run
*!*
// 1. Crie um novo objeto XMLHttpRequest
*/!*
deixe xhr = novo XMLHttpRequest ();

*!*
// 2. Configure: GET-request para o URL /article/.../hello.txt
xhr.open ('GET', '/article/xmlhttprequest/hello.txt');
*/!*

*!*
// 3. Enviar o pedido através da rede
*/!*
xhr.send ();

*!*
// 4. Isso será chamado depois que a resposta for recebida
*/!*
xhr.onload = function () {
se (xhr.status! = 200) {// analise o status HTTP da resposta
// se não for 200, considere um erro
alerta (xhr.status + ':' + xhr.statusText); // por exemplo. 404 não encontrado
} outro {
// mostra o resultado
alerta (xhr.responseText); // responseText é a resposta do servidor
}
};
```

Como podemos ver, existem vários métodos de `XMLHttpRequest` aqui. Vamos cobri-los.

## Setup: "open"

A sintaxe:
`` `js
xhr.open (método, URL, assíncrono, usuário, senha)
```

Esse método geralmente é chamado primeiro após `new XMLHttpRequest`. Especifica os principais parâmetros do pedido:

- `method` - HTTP-method. Normalmente, "GET" ou "POST", mas também podemos usar TRACE / DELETE / PUT e assim por diante.
- `URL` - o URL a solicitar. Pode usar qualquer caminho e protocolo, mas existem limitações entre domínios denominadas "mesma política de origem". Podemos fazer qualquer solicitação para o mesmo `protocolo: // domínio: porta 'de que a página atual vem, mas outros locais são" proibidos "por padrão (a menos que implementem cabeçalhos HTTP especiais, os cobriremos no capítulo [ façam]).
- `async` - se o terceiro parâmetro for explicitamente definido como` false`, a solicitação é síncrona, caso contrário, é assíncrono. Falaremos mais sobre isso neste capítulo em breve.
- `usuário ',` senha' - login e senha para autenticação HTTP básica (se necessário).

Observe que a chamada "aberta", contrariamente ao seu nome, não abre a conexão. Só configura o pedido, mas a atividade de rede só começa com a chamada de `enviar`.

## Envie-o para fora: "enviar"

A sintaxe:
`` `js
xhr.send ([body])
```

Este método abre a conexão e envia a solicitação ao servidor. O parâmetro opcional `body` contém o corpo da solicitação. Alguns métodos de solicitação como `GET` não possuem um corpo. E alguns deles, como "POST", usam `body` para enviar os dados. Veremos exemplos com um corpo no próximo capítulo.


## Cancelar: abortar e tempo limite

Se mudarmos a nossa opinião, podemos encerrar o pedido a qualquer momento. A chamada para `xhr.abort ()` faz isso:

`` `js
xhr.abort (); // encerrar o pedido
```

Também podemos especificar um tempo limite usando a propriedade correspondente:

`` `js
xhr.timeout = 10000;
```

O tempo limite é expresso em ms. Se a solicitação não for bem sucedida dentro do tempo determinado, ela será cancelada automaticamente.

## Eventos: onload, onerror etc

Um pedido é assíncrono por padrão. Em outras palavras, o navegador o envia e permite que outro código JavaScript seja executado.

Depois que o pedido é enviado, `xhr` começa a gerar eventos. Podemos usar as propriedades `addEventListener` ou` on <event> `para lidar com elas, assim como com objetos DOM.

O moderno [especificação] (https://xhr.spec.whatwg.org/#events) lista os seguintes eventos:

- `loadstart` - o pedido começou.
- `progress` - o navegador recebeu um pacote de dados (pode acontecer várias vezes).
- `abort '- o pedido foi abortado por` xhr.abort () `.
- `error` - ocorreu um erro de rede, a solicitação falhou.
- `load` - a solicitação é bem-sucedida, sem erros.
- `timeout` - o pedido foi cancelado devido ao tempo limite (se o tempo limite estiver configurado).
- `loadend` - o pedido é feito (com um erro ou sem ele)
- `readystatechange` - o estado do pedido é alterado (carta de apresentação).

Usando esses eventos, podemos acompanhar o carregamento bem-sucedido (`onload`), os erros ('onerror`) e a quantidade de dados carregados (` onprogress`).

Observe que os erros aqui são "erros de comunicação". Em outras palavras, se a conexão for perdida ou o servidor remoto não responde, então é o erro nos termos de XMLHttpRequest. O status do HTTP incorreto, como 500 ou 404, não é considerado erro.

Aqui está um exemplo mais funcional, com erros e um tempo limite:

`` `html run
<script>
carga de função (url) {
deixe xhr = novo XMLHttpRequest ();
xhr.open ('GET', url);
xhr.timeout = 1000;
xhr.send ();

xhr.onload = function () {
alerta ('Loaded: $ {this.status} $ {this.responseText} `);
};

xhr.onerror = () => alerta ('Erro');

xhr.ontimeout = () => alert ('Timeout!');
}
</ script>

<button onclick = "load ('/ article / xmlhttprequest / hello.txt')"> Carregar </ button>
<button onclick = "load ('/ article / xmlhttprequest / hello.txt? speed = 0')"> Carregar com tempo limite </ button>
<button onclick = "load ('no-such-page')"> Carregar 404 </ button>
<button onclick = "load ('http://example.com')"> Carregar outro domínio </ button>
```

1. O primeiro botão aciona apenas 'onload`, pois carrega o arquivo `hello.txt` normalmente.
2. O segundo botão carrega um URL muito lento, então ele chama apenas 'ontimeout` (porque `xhr.timeout` está configurado).
3. O terceiro botão carrega um URL inexistente, mas também chama `onload` (com" Loaded: 404 "), porque não há erro de rede.
4. O último botão tenta carregar uma página de outro domínio. Isso é proibido, a menos que o servidor remoto concorda explicitamente enviando certos cabeçalhos (para serem cobertos mais tarde), então temos "onerror" aqui. O manipulador 'onerror` também desencadeia em outros casos se iniciarmos uma solicitação e depois cortar a conexão de rede do nosso dispositivo.

## Resposta: status, responseText e outros

Uma vez que o servidor respondeu, podemos receber o resultado nas seguintes propriedades do objeto de solicitação:

`status`
: Código de status HTTP: `200`,` 404`, `403` e assim por diante. Também pode ser `0` se ocorrer um erro.

`statusText`
: Mensagem de status HTTP: geralmente `OK` para` 200`, `Not Found` para` 404`, `Forbidden` para` 403` e assim por diante.

`responseText`
: O texto da resposta do servidor,

Existe outra propriedade que é usada com menos freqüência:

`responseXML`
: Se o servidor retornou XML, fornecendo-o com o cabeçalho correto `Content-type: text / xml`, o navegador criará um documento XML a partir dele. Sobre ele, será possível fazer pedidos `xhr.responseXml.querySelector (" ... ")` e outros.

É raramente usado, porque geralmente não é JSON, mas XML. Ou seja, o servidor retorna JSON como texto, que o navegador se transforma em um objeto chamando `JSON.parse (xhr.responseText)`.

## Solicitações síncronas e assíncronas

Se você definir a opção `async` para` false` no método `open`, a consulta será síncrona.

As chamadas síncronas são extremamente raras, porque bloqueiam a interação com a página antes do download estar completo. Um visitante não pode nem percorrê-lo. Nenhum JavaScript pode ser executado até a chamada síncrona estar concluída - em geral, exatamente as mesmas restrições que o "alerta".

`` `js
// Pedido síncrono
xhr.open ('GET', 'phones.json', *! * false * /! *);

// Envie-o
xhr.send ();
*!*
// ... tudo JavaScript "trava" até a conclusão do pedido
*/!*
```

Se a chamada síncrona demorou muito, o navegador solicitará que você feche a página "pendurada".

Por causa desse bloqueio, verifica-se que você não pode enviar dois pedidos ao mesmo tempo. Além disso, olhando para a frente, observamos que uma série de recursos avançados, como a capacidade de fazer solicitações para outro domínio e especificar um tempo limite, não funcionam no modo síncrono.

De todos os itens acima, já deve ser claro que os pedidos síncronos são usados ​​extremamente raramente, e os assíncronos são quase sempre usados.

Para que a consulta se torne assíncrona, especificamos o parâmetro `async` para ser` true`.

Código JS alterado:

`` `js
var xhr = new XMLHttpRequest ();

xhr.open ('GET', 'phones.json', *! * true * /! *);

xhr.send (); // (1)

*!*
xhr.onreadystatechange = function () {// (3)
se (xhr.readyState! = 4) return;
*/!*

button.innerHTML = 'Готово!';

se (xhr.status! = 200) {
alerta (xhr.status + ':' + xhr.statusText);
} outro {
alerta (xhr.responseText);
}

}

button.innerHTML = 'Загружаю ...'; // (2)
button.disabled = true;
```

Se `third` for especificado como o terceiro argumento` true` (ou se não houver nenhum terceiro argumento), o pedido será executado de forma assíncrona. Isto significa que, após a chamada para `xhr.send ()` na cadeia `(1)` o código não desliga, mas continua funcionando sem problemas, a string `(2)` é executada e o resultado ocorre através do evento `(3)`, vamos estudá-lo um pouco mais tarde.

Exemplo completo em ação:

[codetabs src = "phones-async"]

# Event readystatechange

O evento `readystatechange` ocorre várias vezes durante o processo de envio e recebimento de uma resposta. Você pode ver o "status da consulta atual" na propriedade `xhr.readyState`.

No exemplo acima, usamos apenas o estado `4` (a solicitação está completa), mas há outros.

Todos os estados, por [especificação] (http://www.w3.org/TR/XMLHttpRequest/#states):

`` `js
const sem assinatura curta UNSENT = 0; // estado inicial
const sem assinatura curta OPENED = 1; // вызван open
const não assinado curto HEADERS_RECEIVED = 2; // получены заголовки
const sem assinatura curta LOADING = 3; // o corpo é carregado (o próximo pacote de dados é recebido)
const sem assinatura curta DONE = 4; // o pedido está completo
```

O pedido os passa na ordem `0` ->` 1` -> `2` ->` 3` -> ... -> `3` ->` 4`, o estado `3` é repetido sempre que o próximo pacote de dados é recebido sobre a rede.

O exemplo abaixo mostra a mudança de estados. Nela, o servidor responde à solicitação `digits`, encaminhando uma string de 1000 dígitos por segundo.

[codetabs src = "readystate"]

`` `warn header =" O ponto de interrupção do pacote não está garantido "
Quando o estado `readyState = 3` (recebido o próximo pacote), podemos ver os dados atuais em 'responseText` e, ao que parece, poderia funcionar com esses dados como com a" resposta ao momento atual ".

No entanto, tecnicamente, não controlamos as lacunas entre os pacotes de rede. Se você testar o exemplo acima na rede local, então, na maioria dos navegadores, as quebras serão cada 1000 caracteres, mas, na realidade, o pacote pode ser interrompido em qualquer byte.

Do que é perigoso? Pelo menos em que os caracteres da língua russa codificada em UTF-8 são codificados por dois bytes cada um - e pode ocorrer uma quebra * entre eles *.

Acontece que, com o próximo `readyState` no final do 'responseText`, haverá um meio-símbolo-byte, ou seja, não será uma string válida - parte da resposta! Se o script de alguma maneira não lidar com isso de forma especial, os problemas são inevitáveis.
```

# Cabeçalhos HTTP

`XMLHttpRequest` sabe como indicar seus cabeçalhos no pedido e leia o envio em resposta.

Existem 3 métodos para trabalhar com cabeçalhos HTTP:

`setRequestHeader (nome, valor)`
: Define o cabeçalho `nome` da consulta com o valor 'valor'.

Por exemplo:

`` `js
xhr.setRequestHeader ('Content-Type', 'application / json');
```

`` `warn header =" Restrições de cabeçalho "
Você não pode definir os cabeçalhos que o navegador controla, por exemplo `Referer` ou` Host` e vários outros (lista completa [aqui] (http://www.w3.org/TR/XMLHttpRequest/#the-setrequestheader-method)).

Esta restrição existe para fins de segurança e para controlar a correção da solicitação.
```

`` `` warn header = "O cabeçalho fornecido não pode ser removido"
A peculiaridade de `XMLHttpRequest` é que é impossível cancelar 'setRequestHeader`.

Chamadas repetidas apenas adicionam informações ao cabeçalho, por exemplo:

`` `js
xhr.setRequestHeader ('X-Auth', '123');
xhr.setRequestHeader ('X-Auth', '456');

// o resultado é o cabeçalho:
// X-Auth: 123, 456
```
````

`getResponseHeader (name)`
: Retorna o valor do cabeçalho da resposta `name`, exceto para 'Set-Cookie` e' Set-Cookie2`.

Por exemplo:

`` `js
xhr.getResponseHeader ('Content-Type')
```

`getAllResponseHeaders ()`
: Retorna todos os cabeçalhos de resposta, exceto `Set-Cookie` e 'Set-Cookie2`.

Os cabeçalhos são retornados como uma única linha, por exemplo:

```
Cache-Control: max-age = 31536000
Conteúdo-comprimento: 4260
Content-Type: imagem / png
Data: Sáb, 08 Set 2012 16:53:16 GMT
```

Entre os cabeçalhos é uma quebra de linha em dois caracteres `'\ r \ n" `(não depende do sistema operacional), o valor do cabeçalho é separado por dois pontos com um espaço` `:` `. Este formato é especificado pelo padrão.

Então, se você quiser obter um objeto com pares de cabeçalho-valor, essa linha precisa ser quebrada e processada.

## Timewatch

A duração máxima de uma solicitação assíncrona pode ser especificada pela propriedade `timeout`:

`` `js
xhr.timeout = 30000; // 30 segundos (em milissegundos)
```

Se esse tempo for excedido, a solicitação será encerrada e um evento "ontimeout" será gerado:

`` `js
xhr.ontimeout = function () {
alerta ('Desculpe, o pedido excedeu o tempo máximo');
}
```

## Lista completa de eventos

A [especificação atual] (http://www.w3.org/TR/XMLHttpRequest/#events) fornece os seguintes eventos durante o processamento da solicitação:

- `loadstart` - o pedido é iniciado.
- `progress` - o navegador recebeu o próximo pacote de dados, você pode ler os dados recebidos atuais em 'responseText`.
- `abort '- o pedido foi cancelado chamando` xhr.abort () `.
- `error` - Ocorreu um erro.
- `load` - a solicitação foi bem-sucedida (sem erros) concluída.
- `timeout` - o pedido foi encerrado por tempo limite.
- `loadend` - o pedido foi encerrado (com sucesso ou sem sucesso)

Usando esses eventos, você pode monitorar mais convenientemente o download (`onload`) e o erro (` onerror`), bem como o número de dados baixados (`onprogress`).

Mais cedo, vimos mais um evento - `readystatechange`. Apareceu muito antes, mesmo antes da aparência do padrão atual.

Nos navegadores modernos é possível recusar a favor dos outros, é necessário, como veremos abaixo, levar em consideração os recursos do IE8-9.

## IE8,9: XDomainRequest

No IE8 e no IE9, o suporte para `XMLHttpRequest` é limitado:

- Os eventos não são suportados, exceto `onreadystatechange`.
- O estado `readyState = 3` não é suportado corretamente: o navegador pode gerá-lo apenas uma vez no momento da solicitação e não com cada pacote de dados. Além disso, ele não dá acesso à resposta `responseText` antes de ser totalmente recebido.

O fato é que, quando esses navegadores foram criados, as especificações não foram totalmente elaboradas. Portanto, os desenvolvedores do navegador decidiram adicionar seu próprio objeto `XDomainRequest`, que implementou parte das capacidades do padrão moderno.

E o "XMLHttpRequest" habitual decidiu não tocá-lo, para não quebrar o código existente inadvertidamente.

Falaremos mais detalhadamente sobre `XDomainRequest` no capítulo <info: xhr-crossdomain>. Por enquanto, basta observar que, para obter alguns dos recursos modernos no IE8,9 - em vez de `new XMLHttpRequest ()` você precisa usar `new XDomainRequest`.

Cross-browser:

`` `js
var XHR = ("onload" no novo XMLHttpRequest ())? XMLHttpRequest: XDomainRequest;
var xhr = novo XHR ();
```

Agora, no IE8,9, os eventos 'onload`,' onerror` e `onprogress` são suportados. Isso é para o IE8.9. Para o IE10, o "XMLHttpRequest" usual já está cheio.

### IE9- e cache

Geralmente, as respostas aos pedidos `XMLHttpRequest` são armazenadas em cache, assim como páginas normais.

Mas o IE9 - por padrão armazena em cache todas as respostas que não são fornecidas com um cabeçalho anti-kesh. Outros navegadores não. Para evitar isso, o servidor deve adicionar cabeçalhos anti-cache apropriados em resposta, por exemplo `Cache-Control: no-cache`.

No entanto, recomenda-se usar cabeçalhos do tipo `Expiração ',` Última modificação' e `Cache-Control` em qualquer caso para permitir que o navegador (não necessariamente IE) compreenda o que deve fazer.

Alternativamente, você pode adicionar um parâmetro aleatório ao URL da solicitação que evite o armazenamento em cache.

Por exemplo, em vez de `xhr.open ('GET', 'service', false)` escreva:

`` `js
xhr.open ('GET', *! * 'service? r =' + Math.random () * /! *, false);
```

Por razões históricas, essa maneira de evitar o armazenamento em cache pode ser vista muito, pois os navegadores antigos não lidaram com os cabeçalhos em cache mal. Agora os cabeçalhos do servidor são bem suportados.

## Total

Código típico para uma solicitação GET usando `XMLHttpRequest`:

`` `js
var xhr = new XMLHttpRequest ();

xhr.open ('GET', '/ my / url', true);

xhr.send ();

xhr.onreadystatechange = function () {
se (this.ready State = 4) return;

// no final da consulta estão disponíveis:
// status, statusTexto
// responseText, responseXML (при content-type: text / xml)

se (this.status! = 200) {
// lida com o erro
alerta ('erro:' + (this.status? this.statusText: 'request failed')));
Retorna;
}

// obtenha o resultado deste.responseText ou this.responseXML
}
```

Desmontamos os seguintes métodos `XMLHttpRequest`:

- `open (método, url, async, usuário, senha)`
- `send (body)`
- `aborto ()`
- `setRequestHeader (nome, valor)`
- `getResponseHeader (name)`
- `getAllResponseHeaders ()`

As propriedades do `XMLHttpRequest`:

- `timeout`
- `responseText`
- `responseXML`
- `status`
- `statusText`

Eventos:

- `onreadystatechange`
- `ontimeout`
- `onerror`
- `onload`
- `onprogress`
- `onbort`
- `onloadstart`
- `onloadend`
