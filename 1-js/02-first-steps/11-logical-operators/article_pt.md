# Operadores lógicos

Existem três operadores lógicos em JavaScript: `||` (OR), `&&` (AND), `!` (NOT).

Embora eles sejam chamados de "lógico", eles podem ser aplicados a valores de qualquer tipo, não apenas booleanos. O resultado também pode ser de qualquer tipo.

Vamos ver os detalhes.

[cortar]

## || (OU)

O operador "OR" é representado com dois símbolos de linha vertical:

`` `js
resultado = a || b;
`` `

Na programação clássica, OR lógico é destinado a manipular apenas valores booleanos. Se algum de seus argumentos for "verdadeiro", então ele retorna `true`, caso contrário ele retorna` false`.

Em JavaScript, o operador é um pouco mais complicado e poderoso. Mas primeiro vamos ver o que acontece com os valores booleanos.

Existem quatro possíveis combinações lógicas:

`` `js run
alerta (verdadeiro || verdadeiro); // verdade
alerta (falso || verdadeiro); // verdade
alerta (verdadeiro || falso); // verdade
alerta (falso | | falso); // falso
`` `

Como podemos ver, o resultado é sempre "verdadeiro", exceto no caso em que ambos os operandos são "falso".

Se um operando não for booleano, então ele será convertido em booleano para a avaliação.

Por exemplo, um número `1` é tratado como` true`, um número `0` - como` false`:

`` `js run
se (1 || 0) {// funciona como se (true || false)
alerta ('verdade!');
}
`` `

Na maior parte do tempo, OU `||` é usado em uma instrução `if` para testar se * qualquer * das condições dadas está correta.

Por exemplo:

`` `js run
deixe hora = 9;

*! *
se (hora <10 || hora> 18) {
* /! *
alerta ("O escritório está fechado");
}
`` `

Podemos passar mais condições:

`` `js run
deixe hora = 12;
deixe isWeekend = true;

Se (seu <10 || hora> 18's Weekend) {
alerta ("O escritório está fechado"); // é o fim de semana
}
`` `

## OU procura o primeiro valor verdadeiro

A lógica descrita acima é algo clássica. Agora vamos trazer os recursos "extras" do JavaScipt.

O algoritmo estendido funciona da seguinte forma.

Dado múltiplos valores OR:

`` `js
resultado = valor1 || valor2 || valor3;
`` `

O operador OR `" || "` faz o seguinte:

- Avalie os operandos da esquerda para a direita.
- Para cada operando, converta-o em booleano. Se o resultado for `verdadeiro`, então pare e retorne o valor original desse operando.
- Se todos os outros operandos foram avaliados (ou seja, todos foram falsos), devolva o último operando.

Um valor é retornado em sua forma original, sem a conversão.

Em outras palavras, uma cadeia de OR `" || "` retorna o primeiro valor de verdade ou o último se nenhum valor desse tipo for encontrado.

Por exemplo:

`` `js run
alerta (1 || 0); // 1 (1 é verdade)
alerta (verdadeiro || 'não importa o que'); // (verdadeiro é verdade)

alerta (null || 1); // 1 (1 é o primeiro valor verdadeiro)
alerta (null || 0 || 1); // 1 (o primeiro valor verdadeiro)
alerta (indefinido || null || 0); // 0 (tudo falsamente, retorna o último valor)
`` `

Isso leva a alguns usos interessantes em comparação com um "OR puro, clássico, único booleano".

1. ** Obtendo o primeiro valor verdadeiro da lista de variáveis ​​ou expressões. **

Imagine que temos várias variáveis, que podem conter os dados ou ser "nulas / indefinidas". E precisamos escolher o primeiro com dados.

Podemos usar OR `||` para isso:

`` `js run
deixe currentUser = null;
Deixe DefaultUser = "John";

*! *
let name = currentUser || defaultUser || "sem nome";
* /! *

alerta (nome); // seleciona "John" - o primeiro valor verdadeiro
`` `

Se ambos `currentUser` e` defaultUser` fossem falsos, então, "sem nome" seria o resultado.
2. ** Avaliação de curto-circuito. **

Os operandos podem ser não apenas valores, mas expressões arbitrárias. OU os avalia e os testa da esquerda para a direita. A avaliação pára quando um valor verdadeiro é alcançado e o valor é retornado. O processo é chamado de "avaliação de curto-circuito", porque é tão curto quanto possível da esquerda para a direita.

Isso é claramente visto quando a expressão dada como o segundo argumento tem um efeito colateral. Como uma atribuição variável.

Se executarmos o exemplo abaixo, `x` não seria atribuído:

`` `js run no-embellecer
deixe x;

*! * true * /! * || (x = 1);

alerta (x); // indefinido, porque (x = 1) não avaliado
`` `

... E se o primeiro argumento for "falso", então "OU" continua e avalia o segundo executando assim a tarefa:

`` `js run no-embellecer
deixe x;

*! * false * /! * || (x = 1);

alerta (x); // 1
`` `

Uma tarefa é um caso simples, outros efeitos colaterais podem ser envolvidos.

Como podemos ver, esse caso de uso é uma "maneira mais curta de fazer" se ". O primeiro operando é convertido em booleano e se for falso, então o segundo é avaliado.

Na maioria das vezes, é melhor usar um "regular" `if` para manter o código fácil de entender, mas às vezes isso pode ser útil.

## && (AND)

O operador AND é representado com dois e `` && `:

`` `js
result = a && b;
`` `

Na programação clássica E retorna `true` se ambos os operandos são verdadeiros e` falso ', de outra forma:

`` `js run
alerta (verdadeiro && verdadeiro); // verdade
alerta (falso && verdadeiro); // falso
alerta (true && false); // falso
alerta (falso & & falso); // falso
`` `

Um exemplo com `if`:

`` `js run
deixe hora = 12;
deixe minuto = 30;

se (hora == 12 && minuto == 30) {
alerta ('Tempo é 12:30');
}
`` `

Assim como para OR, qualquer valor é permitido como um operando de AND:

`` `js run
se (1 && 0) {// avaliado como verdadeiro && falso
alerta ("não funcionará, porque o resultado é falso");
}
`` `


## AND busca o primeiro valor falso

Dado vários valores AND'ed:

`` `js
result = value1 && value2 && value3;
`` `

O operador AND `" && "` faz o seguinte:

- Avalie os operandos da esquerda para a direita.
- Para cada operando, converta-o em um booleano. Se o resultado for "falso", pare e retorne o valor original desse operando.
- Se todos os outros operandos foram avaliados (ou seja, todos foram verdadeiros), devolva o último operando.

Em outras palavras, AND retorna o primeiro valor falsy ou o último valor se nenhum deles for encontrado.

As regras acima são semelhantes a OR. A diferença é que AND retorna o primeiro * valor falsy * enquanto OR retorna o primeiro * truthy * one.

Exemplos:

`` `js run
// se o primeiro operando for verdade,
// E retorna o segundo operando:
alerta (1 && 0); // 0
alerta (1 & & 5); // 5

// se o primeiro operando for falso,
// E retorna. O segundo operando é ignorado
alerta (null && 5); // nulo
alerta (0 && "não importa o que"); // 0
`` `

Podemos também passar vários valores seguidos. Veja como o primeiro falso é retornado:

`` `js run
alerta (1 && 2 && null && 3); // nulo
`` `

Quando todos os valores são verdadeiros, o último valor é retornado:

`` `js run
alerta (1 && 2 && 3); // 3, o último
`` `

`` `` smart header = "AND` && `executa antes de '` `
A precedência do operador AND `&&` é maior que OR `||`, então ele é executado antes de OR.

No primeiro código `1 && 0` é calculado primeiro:

`` `js run
alerta (5 || 1 && 0); // 5
`` `
`` ``

Assim como OR, o operador AND `&&` às vezes pode substituir `if`.

Por exemplo:

`` `js run
deixe x = 1;

(x> 0) && alert ('Maior que zero!');
`` `

A ação na parte direita de `&&` seria executada somente se a avaliação chegar a ela. Isto é: somente se `(x> 0)` for verdadeiro.

Então, basicamente, temos um análogo para:

`` `js run
deixe x = 1;

se (x> 0) {
alerta ('maior que zero!');
}
`` `

A variante com `&&` parece ser mais curta. Mas `if` é mais óbvio e tende a ser um pouco mais legível.

Por isso, é recomendável usar cada construção para seu propósito. Use `if` se quisermos se. E use `&&` se queremos AND.

##! (NÃO)

O operador booleano NOT é representado com um sinal de exclamação `"! "`.

A sintaxe é bastante simples:

`` `js
resultado =! valor;
`` `

O operador aceita um único argumento e faz o seguinte:

1. Converte o operando para o tipo booleano: `true / false`.
2. Retorna um valor inverso.

Por exemplo:

`` `js run
alerta (! true); // falso
alerta (! 0); // verdade
`` `

Um duplo NÃO `!!` às vezes é usado para converter um valor em tipo booleano:

`` `js run
alerta ("string não vazio"); // verdade
alerta (!! null); // falso
`` `

Ou seja, o primeiro NÃO converte o valor em booleano e retorna o inverso eo segundo NÃO o inversa novamente. No final, temos uma conversão simples de valor para booleano.

Há uma maneira mais detalhada de fazer o mesmo - uma função 'Booleana' incorporada:

`` `js run
alerta (booleano ("string não vazio")); // verdade
alerta (booleano (nulo)); // falso
`` `
