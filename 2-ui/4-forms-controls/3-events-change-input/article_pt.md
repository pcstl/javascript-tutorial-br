# Eventos: alterar, inserir, cortar, copiar, colar

Vamos discutir vários eventos que acompanham as atualizações de dados.

## Evento: alteração

O evento [change] (http://www.w3.org/TR/html5/forms.html#event-input-change) desencadeia quando o elemento acabou mudando.

Para entradas de texto que significa que o evento ocorre quando perde o foco.

Por exemplo, enquanto estamos digitando no campo de texto abaixo - não há evento. Mas quando movemos o foco em outro lugar, por exemplo, clique em um botão - haverá um evento `change`:

`` `html autorun height = 40 run

<input type = "botão" value = "Button">
`` `

Para outros elementos: `select`,` input type = checkbox / radio` desencadeia logo após a seleção mudar.

## Evento: entrada

O evento `input` desencadeia sempre que um valor é modificado.

Por exemplo:

`` `html autorun height = 40 run
<input type = "text" id = "input"> oninput: <span id = "result"> </ span>
<script>
input.oninput = function () {
result.innerHTML = input.value;
};
</ script>
`` `

Se quisermos lidar com todas as modificações de um `<input>`, esse evento é a melhor escolha.

Ao contrário dos eventos de teclado, ele funciona em qualquer alteração de valor, mesmo aquelas que não envolvem ações de teclado: colar com um mouse ou usar o reconhecimento de fala para ditar o texto.

`` `smart header =" Não é possível impedir nada em `oninput`"
O evento `input` ocorre após o valor ser modificado.

Portanto, não podemos usar `event.preventDefault ()` lá - é muito tarde, não haverá nenhum efeito.
`` `

## Eventos: cortar, copiar, colar

Esses eventos ocorrem ao cortar / copiar / colar um valor.

Eles pertencem à classe [ClipboardEvent] (https://www.w3.org/TR/clipboard-apis/#clipboard-event-interfaces) e fornecem acesso aos dados copiados / colados.

Também podemos usar `event.preventDefault ()` para abortar a ação.

Por exemplo, o código abaixo impede todos esses eventos e mostra o que estamos tentando cortar / copiar / colar:

`` `html autorun height = 40 run
<input type = "text" id = "input">
<script>
input.oncut = input.oncopy = input.onpaste = function (event) {
alerta (event.type + '-' + event.clipboardData.getData ('text / plain'));
retornar falso;
};
</ script>
`` `

Tecnicamente, podemos copiar / colar tudo. Por exemplo, podemos copiar e arquivar no gerenciador de arquivos do sistema operacional e colá-lo.

Existe uma lista de métodos [na especificação] (https://www.w3.org/TR/clipboard-apis/#dfn-datatransfer) para trabalhar com diferentes tipos de dados, ler / gravar na área de transferência.

Mas observe que a área de transferência é uma coisa "global" de nível OS. A maioria dos navegadores permite o acesso de leitura / gravação à área de transferência somente no escopo de certas ações do usuário para a segurança. Também é proibido criar eventos de área de transferência "personalizados" em todos os navegadores, exceto o Firefox.

## Resumo

Eventos de mudança de dados:

| Evento | Descrição | Promoções |
| --------- | ---------- | ------------- |
| `change` | Um valor foi alterado. | Para entradas de texto dispara na perda de foco. |
| `input` | Para entradas de texto em cada mudança. | Tira-se imediatamente ao contrário de "mudar". |
| `cortar / copiar / colar '| Corte / copie / cole ações. | A ação pode ser prevenida. A propriedade `event.clipbordData` dá acesso de leitura / gravação à área de transferência. |
