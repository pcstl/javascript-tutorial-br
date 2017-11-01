# Olá Mundo!

O tutorial que você está lendo é sobre o núcleo da linguagem JavaScript, que é independente da plataforma. Além disso, você aprenderá Node.JS e outras plataformas que o usam.

Mas, precisamos de um ambiente de trabalho para executar nossos scripts e, como este livro já está online, o navegador é uma boa escolha. Limitaremos a quantidade de comandos específicos do navegador (como "alert"), de modo que você não gaste tempo neles se planeja se concentrar em outro ambiente, como Node.JS. Por outro lado, os detalhes do navegador são explicados em detalhes na [próxima parte] (/ ui) do tutorial.

Então, primeiro, vejamos como anexar um script a uma página da Web. Para ambientes do lado do servidor, você pode executá-lo apenas com um comando como "node my.js" `para Node.JS.

## A tag "script"

Os programas JavaScript podem ser inseridos em qualquer parte de um documento HTML com a ajuda da tag `<script>`.

Por exemplo:

```html run height=100
<! DOCTYPE HTML>
<html>

<corpo>

<p> Antes do script ... </ p>

*! *
<script>

</ script>
* /! *

<p> ... Após o script. </ p>

</ body>

</ html>
```

```online
Você pode executar o exemplo clicando no botão "Reproduzir" no canto superior direito.
```

A tag `<script>` contém código JavaScript que é executado automaticamente quando o navegador atende a tag.


## A marcação moderna

A tag `<script>` tem alguns atributos que raramente são usados hoje em dia, mas podemos encontrá-los em código antigo:

O atributo `type`: `<script type=...>`

: O padrão antigo HTML4 requeria que um script explicitasse seu tipo. Normalmente esse tipo era `type ="text / javascript"`. O padrão HTML moderno assume esse `type` por padrão. Nenhum atributo é necessário.

O atributo `language`: `<script language=...>`
: Este atributo deveria especificar a linguagem do script. Atualmente, esse atributo não faz sentido, a linguagem é JavaScript por padrão. Não é necessário usá-lo.

Comentários antes e depois dos scripts.
: Em livros e guias muito antigos, pode-se encontrar comentários dentro de `<script>`, assim:

```html no-beautify
<script type = "text / javascript"> <! -
...
// -> </ script>
```

Esses comentários têm por intenção ocultar o código de navegadores antigo que não entendem a tag `<script> `. Mas todos os navegadores criados nos últimos 15+ anos entendem essa tag corretamente. Nós mencionamos isso aqui, porque esses comentários servem de sinal. Se você vir isso em algum lugar - esse código provavelmente é muito antigo e não vale a pena analisá-lo.


## Scripts externos

Se tivermos muito código JavaScript, podemos colocá-lo em um arquivo separado.

O arquivo de script é anexado ao HTML com o atributo `src`:

```html
<script src = "/caminho/para/script.js"> </ script>
```

Aqui `/caminho/para/script.js` é um caminho absoluto para o arquivo com o script (a partir da raiz do site).

Também é possível fornecer um caminho relativo à página atual. Por exemplo, `src="script.js"significa um arquivo` "script.js" na pasta atual.

Nós também podemos dar um URL completo, por exemplo:

```html
<script src = "https://cdnjs.cloudflare.com/ajax/libs/lodash.js/3.2.0/lodash.js"> </ script>
```

Para anexar vários scripts, use várias tags:

```html
<script src = "/ js / script1.js"> </ script>
<script src = "/ js / script2.js"> </ script>
...
```

```smart
Via de regra, apenas os scripts mais simples são colocados em HTML. Scripts mais complexos residem em arquivos separados.

O benefício de um arquivo separado é que o navegador irá baixá-lo e depois armazenar no seu [cache] (https://en.wikipedia.org/wiki/Web_cache).

Depois disso, outras páginas que desejam o mesmo script irão tirá-lo do cache em vez de baixá-lo. Então, o arquivo é realmente baixado apenas uma vez.

Isso economiza tráfego e torna as páginas mais rápidas.
```

```warn header = "Se` src` estiver configurado, o conteúdo do script será ignorado."
Uma única tag `<script>` não pode ter tanto o atributo `src` como o código dentro.

Isso não funcionará:

```html
<script *! * src * /! * = "file.js">

</ script>
```

Devemos escolher: ou é um `<script src = "...">`ou um `<script>` com código.

O exemplo acima pode ser dividido em dois scripts para funcionar:

```html
<script src = "file.js"> </ script>
<script>

</script>
```
```

## Resumo

- Podemos usar uma tag `<script>` para adicionar código JavaScript à página.
- Os atributos `type` e` language` não são necessários.
- Um script em um arquivo externo pode ser inserido com `<script src =" path / to / script.js "> </ script>`.


Há muito mais para aprender sobre scripts do navegador e sua interação com a página da web. Mas vamos ter em mente que esta parte do tutorial é dedicada ao núcleo JavaScript, então não devemos distrair-nos disso. Nós estaremos usando um navegador como uma maneira de executar o JavaScript, o que é muito conveniente para leitura on-line, mas ainda assim, apenas uma de muitas formas possíveis.
