# A declaração "switch"

Uma declaração `switch` pode substituir várias verificações` if`.

Ele fornece uma maneira mais descritiva de comparar um valor com múltiplas variantes.

[cortar]

## A sintaxe

O `switch` possui um ou mais blocos` case` e ​​um padrão opcional.

Parece assim:

`` `js no-embellecer
mudar (x) {
case 'value1': // if (x === 'value1')
...
[pausa]

case 'value2': // if (x === 'value2')
...
[pausa]

padrão:
...
[pausa]
}
`` `

- O valor de `x` é verificado para uma igualdade rigorosa ao valor do primeiro` case` (ou seja, `value1`), então para o segundo (` value2`) e assim por diante.
- Se a igualdade for encontrada, `switch` começa a executar o código a partir do" caso "correspondente, até o" break "mais próximo (ou até o final do` switch`).
- Se nenhum caso for combinado, o código "padrão" será executado (se existir).

## Um exemplo

Um exemplo de `switch` (o código executado é destacado):

`` `js run
deixe a = 2 + 2;

mudar (a) {
caso 3:
alerta ("muito pequeno");
pausa;
*! *
caso 4:
alerta ('Exatamente!');
pausa;
* /! *
caso 5:
alerta ("Muito grande");
pausa;
padrão:
alerta ("Eu não conheço tais valores");
}
`` `

Aqui o `switch` começa a comparar` a` da primeira variante `case` que é` 3`. A partida falha.

Então `4`. Isso é uma partida, então a execução começa a partir do "caso 4" até a "quebra" mais próxima.

** Se não houver "quebrar", então a execução continua com o próximo caso, sem verificações. **

Um exemplo sem "break":

`` `js run
deixe a = 2 + 2;

mudar (a) {
caso 3:
alerta ("muito pequeno");
*! *
caso 4:
alerta ('Exatamente!');
caso 5:
alerta ("muito grande");
padrão:
alerta ("Eu não conheço tais valores");
* /! *
}
`` `

No exemplo acima, veremos a execução seqüencial de três "alertas":

`` `js
alerta ('Exatamente!');
alerta ("muito grande");
alerta ("Eu não conheço tais valores");
`` `

`` `` cabeçalho inteligente = "Qualquer expressão pode ser um argumento 'switch / case`"
Ambos `switch` e` case` permitem expressões arbitrárias.

Por exemplo:

`` `js run
deixe a = "1";
Deixe b = 0;

interruptor (+ a) {
*! *
caso b + 1:
alerta ("isso corre, porque + a é 1, é exatamente igual a b + 1");
pausa;
* /! *

padrão:
alerta ("isso não é executado");
}
`` `
Aqui `+ a` dá` 1`, comparado com `b + 1` em` case`, e o código correspondente é executado.
`` ``

## Agrupamento de "caso"

Várias variantes de `case` que compartilham o mesmo código podem ser agrupadas.

Por exemplo, se queremos que o mesmo código seja executado para `case 3` e` case 5`:

`` `js run no-embellecer
deixe a = 2 + 2;

mudar (a) {
caso 4:
alerta ('Direito!');
pausa;

*! *
caso 3: // (*) agrupados dois casos
caso 5:
alerta ('Errado!');
alerta ("Por que você não faz uma aula de matemática?");
pausa;
* /! *

padrão:
alerta ('O resultado é estranho. Realmente.');
}
`` `

Agora, ambos `3 e` 5 mostram a mesma mensagem.

A capacidade de "agrupar" casos é um efeito colateral de como `switch / case` funciona sem` break`. Aqui, a execução do `case 3` começa a partir da linha` (*) `e passa por` case 5`, porque não há `break`.

## Type matter

Vamos enfatizar que a verificação da igualdade é sempre rigorosa. Os valores devem ser do mesmo tipo para coincidir.

Por exemplo, vamos considerar o código:

`` `js run
Deixe arg = prompt ("Digite um valor?")
switch (arg) {
caso '0':
caso 1':
alerta ('Um ou zero');
pausa;

caso '2':
alerta ('Dois');
pausa;

caso 3:
alerta ('Nunca executa!');
pausa;
padrão:
alerta ('Um valor desconhecido')
}
`` `

1. Para `0`,` 1`, o primeiro 'alerta' é executado.
2. Para `2 'o segundo' alerta 'é executado.
3. Mas para `3`, o resultado do` prompt` é uma string `" 3 "`, que não é estritamente igual `===` ao número `3`. Então, temos um código morto no `caso 3 '! A variável "padrão" será executada.
