# Operadores condicionais: se, '?'

Às vezes, precisamos realizar diferentes ações com base em uma condição.

Existe a declaração `if` para isso e também o operador condicional (ternário) para avaliação condicional que nos referiremos como o operador" questionário ":` "?" `Por simplicidade.

[cortar]

## A declaração "if"

A instrução "if" obtém uma condição, avalia-a e, se o resultado for `true`, executa o código.

Por exemplo:

`` `js run
let year = prompt ('Em qual ano foi publicada a especificação ECMAScript-2015?', '');

*! *
se (ano == 2015) alerta ('Você está certo!');
* /! *
`` `

No exemplo acima, a condição é uma verificação de igualdade simples: `year == 2015`, mas pode ser muito mais complexa.

Se houver mais de um comando para executar, podemos usar um bloco de código em parênteses de figura:

`` `js
se (ano == 2015) {
alerta ("Isso está correto!");
alerta ("Você é tão inteligente!");
}
`` `

Recomenda-se a utilização de colchetes de cada vez com `if`, mesmo que exista apenas um comando. Isso melhora a legibilidade.

## Conversão booleana

A instrução `if (...)` avalia a expressão entre parênteses e converte-a para o tipo booleano.

Vamos lembrar as regras de conversão do capítulo <info: type-conversions>:

- Um número `0`, uma string vazia` `` `,` null`, `undefined` e` NaN` tornam-se `falso '. Por isso, eles são chamados de valores "falsos".
- Outros valores se tornam "verdadeiros", então eles são chamados de "verdade".

Então, o código sob esta condição nunca seria executado:

`` `js
se (0) {// 0 é falsy
...
}
`` `

... E dentro desta condição - sempre funciona:

`` `js
se (1) {// 1 é verdade
...
}
`` `

Podemos também passar um valor booleano pré-avaliado para `if`, como aqui:

`` `js
Deixe cond = (ano == 2015); // a igualdade é avaliada como verdadeira ou falsa

se (cond) {
...
}
`` `

## A cláusula "else"

A instrução `if` pode conter um bloco opcional" else ". Ele é executado quando a condição está errada.

Por exemplo:
`` `js run
let year = prompt ('Em qual ano foi publicada a especificação ECMAScript-2015?', '');

se (ano == 2015) {
alerta ('Você adivinhou certo!');
} outro {
alerta ('Como você pode estar tão errado?'); // qualquer valor exceto 2015
}
`` `

## Várias condições: "else if"

Às vezes, gostaríamos de testar várias variantes de uma condição. Há uma cláusula `else if` para isso.

Por exemplo:

`` `js run
let year = prompt ('Em qual ano foi publicada a especificação ECMAScript-2015?', '');

se (ano <2015) {
alerta ("Demasiado cedo ...");
} else if (year> 2015) {
alerta ("Demasiado tarde");
} outro {
alerta ('Exatamente!');
}
`` `

No código acima, o JavaScript primeiro verifica `ano <2015`. Se for falso, então vai para a próxima condição `ano> 2015`, e de outra forma mostra o último 'alerta'.

Pode haver mais blocos `else if`. O final `else` é opcional.

## Ternary operator '?'

Às vezes, precisamos atribuir uma variável dependendo de uma condição.

Por exemplo:

`` `js run no-embellecer
Deixe AccessAllowed;
let age = prompt ('Quantos anos você tem?', '');

*! *
se (idade> 18) {
accessAllowed = true;
} outro {
accessAllowed = false;
}
* /! *

alerta (accessAllowed);
`` `

O chamado operador "ternário" ou "ponto de interrogação" nos permite fazer isso de forma mais simples e simples.

O operador é representado por um ponto de interrogação `"? "`. O termo formal "ternário" significa que o operador possui três operandos. Na verdade, é o único e único operador em JavaScript que possui muitos.

A sintaxe é:
`` `js
Deixe resultado = condição? valor1: valor2
`` `

A "condição" é avaliada, se for verdade, então "value1" é retornado, caso contrário - `value2`.

Por exemplo:

`` `js
Deixe accessAllowed = (idade> 18)? verdadeiro falso;
`` `

Tecnicamente, podemos omitir parênteses em torno de "idade> 18". O operador do ponto de interrogação tem uma baixa precedência. Executa após a comparação `>`, de modo que fará o mesmo:

`` `js
// o operador de comparação "idade" 18 "executa primeiro de qualquer maneira
// (não é necessário envolver entre parênteses)
Deixe accessAllowed = age> 18? verdadeiro falso;
`` `

... Mas parênteses tornam o código mais legível. Portanto, é recomendável usá-los.

`` `smart
No exemplo acima, é possível evadir o operador do ponto de interrogação, porque a comparação em si retorna `true / false`:

`` `js
// o mesmo
Deixe accessAllowed = age> 18;
`` `
`` ``

## Múltiplo '?'

Uma seqüência de pontos de interrogação que os operadores ``? "` Permitem retornar um valor que depende de mais de uma condição.

Por exemplo:
`` `js run
let age = prompt ('age?', 18);

Deixe a mensagem = (idade <3)? 'Oi Bebê!' :
(idade <18)? 'Olá!' :
(idade <100)? 'Saudações!' :
'Que idade incomum!';

alerta (mensagem);
`` `

Pode ser difícil, em primeiro lugar, entender o que está acontecendo. Mas depois de um olhar mais próximo, podemos ver que é apenas uma sequência comum de testes.

1. O primeiro ponto de interrogação verifica se `age <3`.
2. Se verdadeiro - retorna `'Oi, baby!'`, Caso contrário - vai após o ponto ``: `` e verifica a idade <18`.
3. Se isso for verdadeiro - retorna `'Olá!'`, Caso contrário - vai após o próximo `` `` `` e verifica a idade <100`.
4. Se isso for verdadeiro - retorna `'Saudações!'`, Caso contrário - vai após o último cólon `": "` e retorna `'Que idade incomum!'`.

A mesma lógica usando `if..else`:

`` `js
se (idade <3) {
mensagem = 'Oi, baby!';
} else if (a <18) {
mensagem = 'Olá!';
} else if (age <100) {
mensagem = 'Saudações!';
} outro {
mensagem = 'Que idade incomum!';
}
`` `

## Uso não tradicional de '?'

Às vezes, o ponto de interrogação ``? '' É usado como um substituto para `if`:

`` `js run no-embellecer
deixe company = prompt ('Qual empresa criou JavaScript?', '');

*! *
(empresa == 'Netscape')?
alerta ('Direito!'): alerta ('Errado');
* /! *
`` `

Dependendo da condição `company == 'Netscape'`, a primeira ou a segunda parte depois que` "?" `É executado e mostra o alerta.

Nós não atribuímos um resultado a uma variável aqui. A idéia é executar código diferente dependendo da condição.

** Não é recomendado usar o operador do ponto de interrogação desta maneira. **

A notação parece ser menor do que `if`, que atrai alguns programadores. Mas é menos legível.

Aqui está o mesmo código com `if` para comparação:

`` `js run no-embellecer
deixe company = prompt ('Qual empresa criou JavaScript?', '');

*! *
se (company == 'Netscape') {
alerta ('Direito!');
} outro {
alerta ('Errado');
}
* /! *
`` `

Nossos olhos digitalizam o código verticalmente. As construções que abrangem várias linhas são mais fáceis de entender do que um longo conjunto de instruções horizontal.

A idéia de um ponto de interrogação `'?'` É retornar um ou outro valor dependendo da condição. Use-o exatamente para isso. Há `if` para executar diferentes ramos do código.
