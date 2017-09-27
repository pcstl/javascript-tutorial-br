
# A sintaxe "nova função"

Há mais uma maneira de criar uma função. É raramente usado, mas às vezes não há alternativa.

[cortar]

## A sintaxe

A sintaxe para criar uma função:

`` `js
deixe func = nova função ('a', 'b', 'return a + b');
`` `

Todos os argumentos de `nova Função 'são strings. Parámetros primeiro e o corpo é o último.

Por exemplo:

`` `js run
let sum = new Function ('arg1', 'arg2', 'return arg1 + arg2');

alerta (soma (1, 2)); // 3
`` `

Se não houver argumentos, haverá apenas corpo:

`` `js run


diga oi(); // Olá
`` `

A principal diferença de outras maneiras que vimos - a função é criada literalmente a partir de uma string, que é passada em tempo de execução.

Todas as declarações anteriores nos exigiram, programadores, para escrever o código da função no script.

Mas `new Function` permite converter qualquer string em uma função, por exemplo, podemos receber uma nova função do servidor e executá-la:

`` `js
Deixe str = ... receber o código do servidor dinamicamente ...

deixe func = nova função (str);
func ();
`` `

Ele é usado em casos muito específicos, como quando recebemos o código do servidor ou para compilar dinamicamente uma função a partir de um modelo. A necessidade disso geralmente surge em estágios avançados de desenvolvimento.

## O encerramento

Normalmente, uma função lembra onde nasceu na propriedade especial `[[Environment]]`. Referencia o ambiente Lexical de onde é criado.

Mas quando uma função é criada usando `new Function`, o` [[Environment]] `referências não é o atual ambiente Lexical, mas sim o global.

`` `js run

function getFunc () {
Deixe value = "test";

*! *
deixe func = nova função ('alerta (valor)');
* /! *

return func;
}

getFunc () (); // erro: o valor não está definido
`` `

Compare com o comportamento regular:

`` `js run
function getFunc () {
Deixe value = "test";

*! *
deixe func = function () {alert (value); };
* /! *

return func;
}

getFunc () (); // *! * "test" * /! *, do ambiente Lexical do getFunc
`` `

Esta característica especial da `nova Função 'parece estranha, mas parece muito útil na prática.

Imagine que realmente temos que criar uma função a partir da string. O código dessa função não é conhecido no momento da escrita do script (é por isso que não usamos funções regulares), mas será conhecido no processo de execução. Podemos recebê-lo do servidor ou de outra fonte.

Nossa nova função precisa interagir com o script principal.

Talvez desejemos que ele possa acessar variáveis ​​locais externas?

Mas o problema é que antes de o JavaScript ser publicado na produção, ele é compactado usando um * minifier * - um programa especial que encolhe o código removendo comentários extras, espaços e - o que é importante, renomeia as variáveis ​​locais para os mais curtos.

Por exemplo, se uma função tem `let userName`, minifier substitui o` let a` (ou outra letra se esta estiver ocupada), e faz em todos os lugares. Isso geralmente é uma coisa segura, porque a variável é local, nada além da função pode acessá-la. E dentro da função minifier substitui cada menção. Minifiers são inteligentes, eles analisam a estrutura do código, não apenas encontrar-e-substituir, então está ok.

... Mas se `nova Função 'pudesse acessar variáveis ​​externas, então não seria possível encontrar` UserName`.

** Mesmo que possamos acessar o ambiente lexical externo em `new Function`, teríamos problemas com minifiers. **

A "característica especial" da "nova função" nos salva de erros.

E impõe um código melhor. Se precisarmos passar algo para uma função, criada por `nova Função ', devemos passá-la explicitamente como argumentos.

A função "soma" realmente faz isso certo:

`` `js run
*! *
let sum = new Function ('a', 'b', 'return a + b;');
* /! *

deixe a = 1, b = 2;

*! *
// os valores externos são passados ​​como argumentos
alerta (soma (a, b)); // 3
* /! *
`` `

## Resumo

A sintaxe:

`` `js
deixe func = nova função (arg1, arg2, ..., corpo);
`` `

Por razões históricas, os argumentos também podem ser dados como uma lista separada por vírgulas.

Estes três significam o mesmo:

`` `js
nova função ('a', 'b', 'return a + b;'); // sintaxe básica
nova função ('a, b', 'return a + b;'); // separados por vírgulas
nova função ('a, b', 'return a + b;'); // separados por vírgulas com espaços
`` `

Funções criadas com `new Function`, têm` [[Environment]] `referenciando o ambiente Lexical global, e não o externo. Portanto, eles não podem usar variáveis ​​externas. Mas isso é realmente bom, porque isso nos poupa de erros. A passagem de parâmetros explícitos é uma coisa muito melhor, arquitetonicamente e não tem problemas com minifiers.
