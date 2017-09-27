# Loops: enquanto e para

Muitas vezes, temos a necessidade de realizar ações similares muitas vezes seguidas.

Por exemplo, quando precisamos produzir produtos de uma lista, um após o outro. Ou simplesmente execute o mesmo código para cada número de 1 a 10.

* Loops * são uma maneira de repetir a mesma parte do código várias vezes.

[cortar]

## O loop "while"

O ciclo `while` possui a seguinte sintaxe:

`` `js
enquanto (condição) {
// código
// chamado "corpo de loop"
}
`` `

Enquanto a `condição 'é` verdadeira', o 'código' do corpo do loop é executado.

Por exemplo, o loop abaixo mostra `i` enquanto` i <3`:

`` `js run
deixe i = 0;
enquanto (i <3) {// mostra 0, depois 1, depois 2
alerta (i);
i ++;
}
`` `

Uma única execução do corpo do loop é chamada de * uma iteração *. O loop no exemplo acima faz três iterações.

Se não houvesse `i ++ 'no exemplo acima, o loop repetiria (em teoria) para sempre. Na prática, o navegador fornece maneiras de parar tais loops e, para o JavaScript do lado do servidor, podemos matar o processo.

Qualquer expressão ou variável pode ser uma condição de loop, não apenas uma comparação. Eles são avaliados e convertidos em booleano por `while`.

Por exemplo, o modo mais curto de escrever `while (i! = 0)` pode ser `while (i)`:

`` `js run
deixe i = 3;
*! *
enquanto (i) {// quando eu me tornar 0, a condição torna-se falsa, e o loop pára
* /! *
alerta (i);
Eu--;
}
`` `

`` `` cabeçalho inteligente = "Brackets não são necessários para um corpo de uma única linha"
Se o corpo do loop tiver uma única declaração, podemos omitir os colchetes `{...}`:

`` `js run
deixe i = 3;
*! *
enquanto (i) alerta (i--);
* /! *
`` `
`` ``

## O loop "do..while"

A verificação da condição pode ser movida * abaixo * o corpo do loop usando a sintaxe `do..while`:

`` `js
Faz {
// corpo do loop
} enquanto (condição);
`` `

O loop executará primeiro o corpo, depois verifique a condição e, enquanto for verdade, execute-o novamente e novamente.

Por exemplo:

`` `js run
deixe i = 0;
Faz {
alerta (i);
i ++;
} enquanto (i <3);
`` `

Esta forma de sintaxe raramente é usada, exceto quando você deseja que o corpo do loop seja executado ** pelo menos uma vez **, independentemente da condição ser verdadeira. Normalmente, a outra forma é preferida: `while (...) {...}`.

## O loop "for"

O loop `for` é o mais utilizado.

Parece assim:

`` `js
para (begin; condition; step) {
// ... loop body ...
}
`` `

Vamos aprender o significado dessas peças por exemplo. O loop abaixo é executado `alert (i)` for `i` de` 0` para (mas não incluindo) `3`:

`` `js run
para (vamos i = 0; i <3; i ++) {// mostra 0, depois 1, depois 2
alerta (i);
}
`` `

Vamos examinar a parte de declaração for for por parte:

| parte | | |
| ------- | ---------- | ------------------------------ ---------------------------------------------- |
| comece | `i = 0` | Executa uma vez ao entrar no loop. |
| condição | `i <3` | Verificado antes de cada iteração de loop, se falhar, o loop pára. |
| passo | `i ++` | Executa após o corpo em cada iteração, mas antes da verificação da condição. |
| corpo | `alert (i)` | Executa uma e outra vez, enquanto a condição é verdadeira |


O algoritmo de loop geral funciona assim:
`` `
Comece a começar
→ (se condição → executar o corpo e executar o passo)
→ (se condição → executar o corpo e executar o passo)
→ (se condição → executar o corpo e executar o passo)
→ ...
`` `

Se você é novo em loops, talvez seja útil se você voltar ao exemplo e reproduzir como ele é executado passo a passo em um pedaço de papel.

Aqui está o que exatamente acontece no nosso caso:

`` `js
// para (deixe i = 0; i <3; i ++) alert (i)

// execute begin
deixe i = 0
// se condição → executar corpo e executar a etapa
se (i <3) {alert (i); i ++}
// se condição → executar corpo e executar a etapa
se (i <3) {alert (i); i ++}
// se condição → executar corpo e executar a etapa
se (i <3) {alert (i); i ++}
// ... termine, porque agora eu == 3
`` `

`` `` cabeçalho inteligente = "declaração de variável em linha"
Aqui, a variável "contador" i é declarada diretamente. Isso é chamado de declaração de variável "inline". Tais variáveis ​​são visíveis apenas dentro do loop.

`` `js run
para (*! * let * /! * i = 0; i <3; i ++) {
alerta (i); // 0, 1, 2
}
alerta (i); // erro, nenhuma variável desse tipo
`` `

Em vez de definir uma variável, podemos usar uma existente:

`` `js run
deixe i = 0;

para (i = 0; i <3; i ++) {// use uma variável existente
alerta (i); // 0, 1, 2
}

alerta (i); // 3, visível, porque declarou fora do loop
`` `

`` ``


### Saltar partes

Qualquer parte do `for` pode ser ignorada.

Por exemplo, podemos omitir "começar" se não precisarmos fazer nada no início do loop.

Como aqui:

`` `js run
deixe i = 0; // nós já declaramos e atribuímos

para (; i <3; i ++) {// não há necessidade de "começar"
alerta (i); // 0, 1, 2
}
`` `

Também podemos remover a parte `step`:

`` `js run
deixe i = 0;

para (; i <3;) {
alerta (i);
}
`` `

O loop tornou-se idêntico a `while (i <3)`.

Podemos realmente remover tudo, criando assim um loop infinito:

`` `js
para (;;) {
// repete sem limites
}
`` `

Por favor, note que os dois vírgulas `for` `` `devem estar presentes, caso contrário, seria um erro de sintaxe.

## Breaking the loop

Normalmente, o loop sai quando a condição torna-se falsa.

Mas podemos forçar a saída a qualquer momento. Existe uma diretriz especial de "quebrar" para isso.

Por exemplo, o loop abaixo pede ao usuário uma série de números, mas "quebra" quando nenhum número é inserido:

`` `js
deixe soma = 0;

enquanto (verdadeiro) {

Deixe o valor = + prompt ("Digite um número", '');

*! *
se (! value) break; // (*)
* /! *

soma + = valor;

}
alerta ('Soma:' + soma);
`` `

A diretiva `break` está ativada na linha` (*) `se o usuário entrar em uma linha vazia ou anula a entrada. Ele pára o loop imediatamente, passando o controle para a primeira linha após o loop. Ou seja, "alerta".

A combinação de "loop infinito +` break` conforme necessário "é ótima para situações em que a condição deve ser verificada não no início / fim do loop, mas no meio, ou mesmo em vários lugares do corpo.

## Continue para a próxima iteração [#continue]

A diretiva `continuação 'é uma" versão mais leve "do` break`. Não pára todo o loop. Em vez disso, interrompe a iteração atual e força o loop para iniciar uma nova (se a condição permitir).

Podemos usá-lo se terminarmos a iteração atual e gostaríamos de passar para a próxima.

O loop acima usa `continue` para produzir apenas valores ímpares:

`` `js run no-embellecer
para (vamos i = 0; i <10; i ++) {

// se for verdade, ignore a parte restante do corpo
*! * if (i% 2 == 0) continue; * /! *

alerta (i); // 1, então 3, 5, 7, 9
}
`` `

Para valores pares de `i`, a diretiva` continue` pára a execução do corpo, passando o controle para a próxima iteração de `for` (com o próximo número). Então, o `alert` é chamado de valores ímpares.

`` `` cabeçalho inteligente = "A diretiva` continuar 'ajuda a diminuir o nível de aninhamento "
Um loop que mostra valores estranhos pode ser assim:

`` `js
para (vamos i = 0; i <10; i ++) {

se (i% 2) {
alerta (i);
}

}
`` `

Do ponto de vista técnico, é idêntico ao exemplo acima. Certamente, podemos simplesmente conter o código no bloco `if` em vez de` continuar '.

Mas, como efeito colateral, obtivemos mais um nível de nidificação entre colchetes. Se o código dentro de `if` for maior que algumas linhas, isso pode diminuir a legibilidade geral.
`` ``

`` `` warn header = "No` break / continue` no lado direito de '?' "
Observe que as construções de sintaxe que não são expressões não podem ser usadas em `'?'`. Em particular, as diretrizes `break / continuo 'não são permitidas.

Por exemplo, se tomarmos esse código:

`` `js
se (i> 5) {
alerta (i);
} outro {
continuar;
}
`` `

... E reescreva-o usando um ponto de interrogação:


`` `js no-embellecer
(i> 5)? alerta (i): *! * continue * /! *; // continua não permitido aqui
`` `

... Então, ele pára de funcionar. O código como este dará um erro de sintaxe:


Esse é apenas outro motivo para não usar um operador de marca de pergunta ``? '`Em vez de` if`.
`` ``

## Etiquetas para quebrar / continuar

Às vezes, precisamos sair de vários loops aninhados ao mesmo tempo.

Por exemplo, no código abaixo, fazemos um loop sobre `i` e` j 'para as coordenadas `(i, j)` de `(0,0)` para `(3,3)`:

`` `js run no-embellecer
para (vamos i = 0; i <3; i ++) {

para (let j = 0; j <3; j ++) {

Deixe entrada = prompt (`Valor em coords ($ {i}, $ {j})`, '');

// e se eu quiser sair daqui até a Feito (abaixo)?

}
}

alerta ('Feito!');
`` `

Precisamos de uma maneira de parar o processo se o usuário cancelar a entrada.

O "break" comum depois da "entrada" só quebraria o circuito interno. Isso não é suficiente. As etiquetas vêm para o resgate.

Um * rótulo * é um identificador com dois pontos antes de um loop:
`` `js
labelName: para (...) {
...
}
`` `

A declaração `break <labelName>` no loop explode para o rótulo.

Como aqui:

`` `js run no-embellecer
*! * exterior: * /! * for (vamos i = 0; i <3; i ++) {

para (let j = 0; j <3; j ++) {

Deixe entrada = prompt (`Valor em coords ($ {i}, $ {j})`, '');

// se for uma string vazia ou cancelada, então saia de ambos os loops
se (! input) *! * break outer * /! *; // (*)

// faça algo com o valor ...
}
}
alerta ('Feito!');
`` `

No código acima `break outer` olha para cima para o rótulo chamado` outer` e sai do loop.

Então, o controle vai direto de `(*)` para 'alert (' Done! ') `.

Também podemos mover o rótulo para uma linha separada:

`` `js no-embellecer
exterior:
para (vamos i = 0; i <3; i ++) {...}
`` `

A diretiva `continuação 'também pode ser usada com um rótulo. Nesse caso, a execução salta para a próxima iteração do loop rotulado.

`` `` cabeçalho de aviso = "Etiquetas não são \" goto \ ""
As etiquetas não nos permitem saltar para um lugar arbitrário de código.

Por exemplo, é impossível fazer isso:
`` `js
etiqueta de ruptura; // salta para rotular? Não.

etiqueta: para (...)
`` `

A chamada para 'break / continue' só é possível no interior do loop, e o rótulo deve estar em algum lugar a partir da diretiva.
`` ``

## Resumo

Cobrimos 3 tipos de loops:

- `while` - A condição é verificada antes de cada iteração.
- `do..while` - A condição é verificada após cada iteração.
- `for (;;)` - A condição é verificada antes de cada iteração, configurações adicionais disponíveis.

Para fazer um ciclo "infinito", geralmente a construção `while (true)` é usada. Esse loop, como qualquer outro, pode ser interrompido com a diretiva `break`.

Se não desejarmos fazer nada na iteração atual e gostaríamos de encaminhar para o próximo, a diretiva `continue 'faz isso.

`Break / continue` suporta etiquetas antes do loop. Um rótulo é o único caminho para `break / continue` para escapar do nidificação e ir para o loop externo.
