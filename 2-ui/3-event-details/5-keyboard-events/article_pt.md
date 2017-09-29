# Teclado: keydown e keyup

Antes de entrar no teclado, note que, em dispositivos modernos, existem outras formas de "inserir algo". Por exemplo, as pessoas usam reconhecimento de fala (especialmente em dispositivos móveis) ou copiar / colar com o mouse.

Então, se quisermos acompanhar qualquer entrada em um campo `<input>`, então os eventos de teclado não são suficientes. Há outro evento chamado `input` para lidar com mudanças de um campo` <input> `, por qualquer meio. E pode ser uma escolha melhor para essa tarefa. Vamos abordá-lo mais adiante no capítulo <info: events-change-input>.

Eventos de teclado devem ser usados ​​quando queremos lidar com ações de teclado (o teclado virtual também conta). Por exemplo, para reagir nas teclas de seta `key: Up` e` key: Down` ou hotkeys (incluindo combinações de teclas).

[cortar]


## Teststand [# keyboard-test-stand]

`` `desconectado
Para entender melhor os eventos do teclado, você pode usar o [testingtand] (sandbox: keyboard-dump).
`` `

`` `online
Para entender melhor os eventos do teclado, você pode usar o ensaio abaixo.

Experimente diferentes combinações de teclas no campo de texto.

[codetabs src = "teclado-despejo" height = 480]
`` `


## Keydown e keyup

Os eventos `keydown` ocorre quando uma tecla é pressionada para baixo e, em seguida,` keyup` - quando é lançado.

### event.code e event.key

A propriedade `key` do objeto de evento permite obter o caractere, enquanto a propriedade` code` do objeto de evento permite obter o "código de chave física".

Por exemplo, a mesma chave `chave: Z 'pode ser pressionada com ou sem` Shift`. Isso nos dá dois caracteres diferentes: `z` em minúsculas e maiúsculas `Z`.

O `event.key` é exatamente o personagem, e será diferente. Mas `event.code` é o mesmo:

| Chave | `event.key` | `event.code` |
| -------------- | ------------- | -------------- |
| `chave: Z` |` z` (minúsculas) | `KeyZ` |
| `chave: Shift + Z` |` Z` (maiúscula) | `KeyZ` |


Se um usuário trabalha com diferentes idiomas, então mudar para outro idioma faria um personagem totalmente diferente em vez de `" Z "`. Isso se tornará o valor de `event.key`, enquanto` event.code` é sempre o mesmo: `" KeyZ "`.

`` `cabeçalho inteligente =" \ "KeyZ \" e outros códigos de chave "
Cada tecla possui o código que depende da sua localização no teclado. Códigos de chave descritos na [Especificação do código de eventos UI] (https://www.w3.org/TR/uievents-code/).

Por exemplo:
- As teclas de letras possuem códigos `" Chave <letra> "`: `" KeyA "`, `" KeyB "` etc.
- As teclas de dígitos possuem códigos: `" Dígito <número> "`: `" Dígito0 "`, `" Dígito 1 "etc.
- As teclas especiais são codificadas por seus nomes: `" Enter "`, `Backspace '',` "Tab" `etc.

Existem vários layouts de teclado difundidos e a especificação fornece códigos-chave para cada um deles.

Consulte [seção alfanumérica da especificação] (https://www.w3.org/TR/uievents-code/#key-alphanumeric-section) para obter mais códigos, ou apenas tente o [testingtand] (# teclado-test-stand ) acima.
`` `

`` `warn header =" O caso é importante: `\" KeyZ \ "`, não `\" keyZ \ "` "
Parece óbvio, mas as pessoas ainda cometem erros.

Por favor, evite mistypes: é `KeyZ`, não` keyZ`. A verificação como `event.code ==" keyZ "` não funcionará: a primeira letra da "Chave" deve ser maiúscula.
`` `


E se uma chave não dá nenhum caracter? Por exemplo, `chave: Shift` ou` key: F1` ou outros. Para as teclas `event.key` é aproximadamente o mesmo que` event.code`:


| Chave | `event.key` | `event.code` |
| -------------- | ------------- | -------------- |
| `chave: F1` |` F1` | `F1` |
| `key: Backspace` |` Backspace` | `Backspace` |
| `chave: Shift` |` Shift` | `ShiftRight` ou` ShiftLeft` |

Observe que `event.code` especifica exatamente qual tecla é pressionada. Por exemplo, a maioria dos teclados tem duas teclas `chave: Shift`: no lado esquerdo e direito. O `event.code` nos diz exatamente qual foi pressionado e` event.key` é responsável pelo "significado" da chave: o que é (um "Shift").

Digamos que queremos lidar com uma tecla de atalho: `chave: Ctrl + Z` (ou` chave: Cmd + Z` para Mac). A maioria dos editores de texto conecta a ação "Desfazer" nela. Podemos definir um ouvinte no `keydown` e verificar qual tecla é pressionada - para detectar quando temos a tecla de atalho.

Por favor, responda a pergunta - em tal ouvinte, devemos verificar o valor de `event.key` ou` event.code`?

Por favor, pause e responda.

Tentou sua opinião?

Se você entender, a resposta é, é claro, `event.code`, porque não queremos` event.key`. O valor de `event.key` pode mudar dependendo do idioma ou` CapsLock` habilitado. O valor de `event.code` é estritamente vinculado à chave, então aqui vamos:

`` `js run
document.addEventListener ('keydown', function (event) {
se (event.code == 'KeyZ' && (event.ctrlKey || event.metaKey)) {
alerta ('Desfazer!')
}
});
`` `

## Auto-repeat

Se uma tecla estiver sendo pressionada durante um tempo longo, ele começa a repetir: o `keydown` desencadeia repetidas vezes e, quando é lançado, finalmente conseguimos` keyup`. Então, é normal ter muitos "keydown" e um único `keyup`.

Para todas as teclas de repetição, o objeto de evento tem a propriedade `event.repeat` definida como` true`.


## Ações padrão

As ações padrão variam, pois há muitas coisas possíveis que podem ser iniciadas pelo teclado.

Por exemplo:

- Um caractere aparece na tela (o resultado mais óbvio).
- Um caractere é excluído (tecla 'chave: apagar').
- A página está rodada (tecla `chave: página).
- O navegador abre a caixa de diálogo "Salvar página" (`chave: Ctrl + S`)
-  ...e assim por diante.

A prevenção da ação padrão no `keydown` pode cancelar a maioria deles, com exceção das chaves especiais baseadas no sistema operacional. Por exemplo, na chave do Windows `: Alt + F4` fecha a janela atual do navegador. E não há como detê-lo, impedindo a ação padrão em JavaScript.

Por exemplo, o `<input>` abaixo espera um número de telefone, portanto, ele não aceita as teclas, exceto os dígitos, `+`, `()` ou `-`:

`` `html autorun height = 60 run
<script>
função checkPhoneKey (chave) {
retorno (chave> = '0' && chave <= '9') || chave == '+' || chave == '(' || key == ')' || chave == '-';
}
</ script>
<input *! * onkeydown = "return checkPhoneKey (event.key)" * /! * placeholder = "Telefone, por favor" digite = "tel">
`` `

Observe que as teclas especiais como `key: Backspace`,` key: Left`, `key: Right`,` key: Ctrl + V` não funcionam na entrada. Esse é um efeito colateral do filtro rigoroso `checkPhoneKey`.

Vamos relaxar um pouco:


`` `html autorun height = 60 run
<script>
função checkPhoneKey (chave) {
retorno (chave> = '0' && chave <= '9') || chave == '+' || chave == '(' || key == ')' || chave == '-' ||
chave == 'ArrowLeft' || chave == 'ArrowRight' || chave == 'Apagar' || chave == 'Backspace';
}
</ script>
<input onkeydown = "return checkPhoneKey (event.key)" placeholder = "Telefone, por favor" digite = "tel">
`` `

Agora, setas e eliminação funcionam bem.

... Mas ainda podemos inserir qualquer coisa usando um mouse e clique com o botão direito do mouse + Colar. Portanto, o filtro não é 100% confiável. Podemos deixar isso assim, porque a maior parte do tempo funciona. Ou uma abordagem alternativa seria rastrear o evento `input` - ele dispara após qualquer modificação. Lá, podemos verificar o novo valor e destacá-lo / modificá-lo quando é inválido.

## Legado

No passado, houve um evento `keypress`, e também` keyCode`, `charCode`,` `` do objeto de evento.

Havia tantas incompatibilidades de navegador que os desenvolvedores da especificação decidiram depreciar todos eles. O código antigo ainda funciona, já que o navegador continua apoiando-os, mas não há necessidade de usar esses mais.

Houve momentos em que este capítulo incluiu sua descrição detalhada. Mas a partir de agora podemos esquecer sobre aqueles.


## Resumo

Pressionar uma tecla sempre gera um evento de teclado, seja teclas de símbolo ou teclas especiais como `chave: Shift` ou` chave: Ctrl` e assim por diante. A única exceção é a chave 'chave: Fn` que às vezes apresenta em um teclado de laptop. Não há evento de teclado para isso, porque geralmente é implementado em um nível inferior ao sistema operacional.

Eventos de teclado:

- `keydown` - ao pressionar a tecla (auto-repete se a tecla for pressionada por muito tempo)
- `keyup` - ao soltar a tecla.

Propriedades do evento do teclado principal:

- `code` - o" código da chave "(` "KeyA" `,` "ArrowLeft" `e assim por diante), específico para a localização física da tecla no teclado.
- `key` - o caractere (` "A" `,` "a" e assim por diante), para as teclas de não-caractere geralmente tem o mesmo valor que `` `` code`.

No passado, os eventos de teclado às vezes eram usados ​​para rastrear a entrada do usuário nos campos do formulário. Isso não é confiável, porque a entrada pode vir de várias fontes. Nós temos eventos `input` e` change` para lidar com qualquer entrada (abordada mais adiante no capítulo <info: events-change-input>). Eles desencadeiam após qualquer entrada, incluindo reconhecimento de mouse ou fala.

Devemos usar eventos de teclado quando realmente queremos teclado. Por exemplo, para reagir em hotkeys ou teclas especiais.
