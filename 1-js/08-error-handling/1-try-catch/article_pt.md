# Manipulação de erros, "try..catch"

Não importa o quão grande estamos na programação, às vezes nossos scripts têm erros. Eles podem ocorrer por causa de nossos erros, uma entrada de usuário inesperada, uma resposta errada do servidor e por outros mil outros motivos.

Normalmente, um script "morre" (pára imediatamente) em caso de erro, imprimindo-o para o console.

Mas há uma construção de sintaxe `try..catch` que permite" pegar "erros e, em vez de morrer, fazer algo mais razoável.

[cortar]

## A sintaxe "try..catch"

A construção `try..catch` tem dois blocos principais:` try`, e depois `catch`:

`` `js
experimentar {

// código ...

} catch (err) {

// manipulação de erros

}
`` `

Funciona assim:

1. Primeiro, o código em `try {...}` é executado.
2. Se não houve erros, então, "catch (err)" é ignorado: a execução atinge o final do `try` e depois salta sobre` catch`.
3. Se ocorrer um erro, a execução da "tentativa" é interrompida e o controle flui para o início de `catch (err)`. A variável `err` (pode usar qualquer nome para ele) contém um objeto de erro com detalhes sobre o que aconteceu.

! [] (try-catch-flow.png)

Então, um erro dentro do bloco `try {...}` não mata o script: temos a chance de lidar com o `catch`.

Vamos ver mais exemplos.

- Um exemplo sem erro: mostra `alert`` (1) `e` (2) `:

`` `js run
experimentar {

alerta ('Início da tentativa executiva'); // *! * (1) <- * /! *

// ... sem erros aqui

alerta ("Fim das corridas de teste"); // *! * (2) <- * /! *

} catch (err) {

alerta ('Catch é ignorado, porque não há erros'); // (3)

}

alerta ("... então a execução continua");
`` `
- Um exemplo com um erro: mostra `(1)` e `(3)`:

`` `js run
experimentar {

alerta ('Início da tentativa executiva'); // *! * (1) <- * /! *

*! *
lalala; // erro, a variável não está definida!
* /! *

alerta ('Fim da tentativa (nunca alcançado)'); // (2)

} catch (err) {

alerta ('Erro ocorreu!'); // *! * (3) <- * /! *

}

alerta ("... então a execução continua");
`` `


`` `'ar cabeçalho =" `try..catch` funciona apenas para erros de tempo de execução"
Para `try..catch` para funcionar, o código deve ser executável. Em outras palavras, deve ser um JavaScript válido.

Não funcionará se o código for sintaticamente errado, por exemplo, possui parênteses incomparáveis:

`` `js run
experimentar {
{{{{{{{{{{{{{
} catch (e) {
alerta ("O motor não consegue entender esse código, é inválido");
}
`` `

O mecanismo de JavaScript primeiro lê o código e depois o executa. Os erros que ocorrem na frase de leitura são chamados de erros de "parse-time" e são irrecuperáveis ​​(de dentro desse código). Isso porque o motor não consegue entender o código.

Então, `try..catch` só pode lidar com erros que ocorrem no código válido. Esses erros são chamados de "erros de tempo de execução" ou, às vezes, "exceções".
`` ``


`` `'ar cabeçalho =" `try..catch` funciona de forma síncrona"
Se uma exceção ocorrer em um código "agendado", como em `setTimeout`, então` try..catch` não irá capturá-lo:

`` `js run
experimentar {
setTimeout (function () {
noSuchVariable; // script morrerá aqui
}, 1000);
} catch (e) {
alerta ("não funcionará");
}
`` `

Isso porque `try..catch` realmente envolve a chamada` setTimeout` que agende a função. Mas a função em si é executada mais tarde, quando o motor já deixou a construção `try..catch`.

Para capturar uma exceção dentro de uma função agendada, `try..catch` deve estar dentro dessa função:
`` `js run
setTimeout (function () {
experimentar {
noSuchVariable; // try..catch lida com o erro!
} catch (e) {
alerta ("o erro é pego aqui!");
}
}, 1000);
`` `
`` ``

## Objeto de erro

Quando ocorre um erro, o JavaScript gera um objeto contendo os detalhes sobre ele. O objeto é então passado como um argumento para `catch`:

`` `js
experimentar {
// ...
} catch (err) {// <- o "objeto de erro", poderia usar outra palavra em vez de errar
// ...
}
`` `

Para todos os erros internos, o objeto de erro dentro do bloco `catch` possui duas propriedades principais:

`nome`
: Nome do erro. Para uma variável indefinida que é `" ReferenceError "`.

`mensagem '
: Mensagem textual sobre detalhes de erro.

Existem outras propriedades não padrão disponíveis na maioria dos ambientes. Um dos mais utilizados e suportados é:

`stack`
: Pilha de chamada atual: uma string com informações sobre a seqüência de chamadas aninhadas que levaram ao erro. Usado para fins de depuração.

Por exemplo:

`` `js não são confiáveis
experimentar {
*! *
lalala; // erro, a variável não está definida!
* /! *
} catch (err) {
alerta (err.name); // ReferenceError
alerta (error.message); // lalala não está definido
alerta (err.stack); // ReferenceError: lalala não está definido em ...

// Também pode mostrar um erro como um todo
// O erro é convertido em seqüência de caracteres como "nome: mensagem"
alerta (erro); // ReferenceError: lalala não está definido
}
`` `


## Usando "try..catch"

Vamos explorar um caso de uso da vida real de `try..catch`.

Como já sabemos, o método JavaScript aceita [JSON.parse (str)] (mdn: js / JSON / parse) para ler os valores codificados em JSON.

Geralmente, ele é usado para decodificar os dados recebidos através da rede, do servidor ou de outra fonte.

Nós recebemos e chamamos `JSON.parse`, assim:

`` `js run
Deixe json = '{"name": "John", "age": 30}'; // dados do servidor

*! *
Deixe o usuário = JSON.parse (json); // converte a representação de texto para o objeto JS
* /! *

// agora o usuário é um objeto com propriedades da string
alerta (user.name); // John
alerta (user.age); // 30
`` `

Informações mais detalhadas sobre o JSON que você pode encontrar no capítulo <info: json>.

** Se `json` estiver mal formado,` JSON.parse` gera um erro, então o script "morre". **

Devemos estar satisfeitos com isso? Claro que não!

Desta forma, se algo estiver errado com os dados, o visitante nunca saberá isso (a menos que ele abra o console do desenvolvedor). E realmente as pessoas realmente não gostam quando algo "simplesmente morre" sem qualquer mensagem de erro.

Vamos usar `try..catch` para lidar com o erro:

`` `js run
Deixe json = "{bad json}";

experimentar {

*! *
Deixe o usuário = JSON.parse (json); // <- quando ocorre um erro ...
* /! *
alerta (user.name); // não funciona

} catch (e) {
*! *
// ... a execução salta aqui
alerta ("Nossas desculpas, os dados têm erros, tentaremos solicitá-lo mais uma vez.");
alerta (e.name);
alerta (e.message);
* /! *
}
`` `

Aqui, usamos o bloco `catch` apenas para mostrar a mensagem, mas podemos fazer muito mais: uma nova solicitação de rede, sugerimos uma alternativa ao visitante, envie as informações sobre o erro para uma instalação de registro ... Tudo muito melhor do que apenas morrendo.

## Lançando seus próprios erros

E se `json` for sintaticamente correto ... Mas não tem uma propriedade necessária de" nome "?

Como isso:

`` `js run
Deixe json = '{"age": 30}'; // dados incompletos

experimentar {

Deixe o usuário = JSON.parse (json); // <- sem erros
*! *
alerta (user.name); // nenhum nome!
* /! *

} catch (e) {
alerta ("não executa");
}
`` `

Aqui `JSON.parse` é executado normalmente, mas a ausência de` "name" `é realmente um erro para nós.

Para unificar o tratamento de erros, usaremos o operador `throw`.

### Operador "Lançar"

O operador `throw` gera um erro.

A sintaxe é:

`` `js
lance <objeto de erro>
`` `

Tecnicamente, podemos usar qualquer coisa como um objeto de erro. Isso pode ser até mesmo um primitivo, como um número ou uma string, mas é melhor usar objetos, preferencialmente com as propriedades `name` e` message` (para ficar um pouco compatível com erros internos).

O JavaScript tem muitos construtores integrados para erros padrão: `Error`,` SyntaxError`, `ReferenceError`,` TypeError` e outros. Podemos usá-los para criar objetos de erro também.

Sua sintaxe é:

`` `js
Deixe erro = novo erro (mensagem);
// ou
Deixe erro = novo SyntaxError (mensagem);
Deixe erro = novo ReferenceError (mensagem);
// ...
`` `

Para erros internos (não para nenhum objeto, apenas para erros), a propriedade `name` é exatamente o nome do construtor. E 'mensagem' é tirada do argumento.

Por exemplo:

`` `js run
Deixe erro = novo erro ("As coisas acontecem o_O");

alerta (error.name); // Erro
alerta (error.message); // As coisas acontecem o_O
`` `

Vamos ver que tipo de erro `JSON.parse` gera:

`` `js run
experimentar {
JSON.parse ("{bad json o_O}");
} catch (e) {
*! *
alerta (e.name); // Erro de sintaxe
* /! *
alerta (e.message); // token inesperado no JSON na posição 0
}
`` `

Como podemos ver, isso é um `SyntaxError`.

... E, no nosso caso, a ausência de `nome` pode ser tratada como um erro de sintaxe também, assumindo que os usuários devem ter um` "nome" `.

Então vamos jogá-lo:

`` `js run
Deixe json = '{"age": 30}'; // dados incompletos

experimentar {

Deixe o usuário = JSON.parse (json); // <- sem erros

se (! user.name) {
*! *
lance novo SyntaxError ("Dados incompletos: sem nome"); // (*)
* /! *
}

alerta (user.name);

} catch (e) {
alerta ("Erro JSON:" + e.message); // Erro JSON: dados incompletos: sem nome
}
`` `

Na linha `(*)` o operador `throw` gera` SyntaxError` com a "mensagem" fornecida, da mesma forma que o JavaScript se geraria. A execução do `try` pára imediatamente e o fluxo de controle entra em 'catch'.

Agora, `catch` tornou-se um único lugar para todo o tratamento de erros: tanto para` JSON.parse` e ​​outros casos.

## Rethrowing

No exemplo acima, usamos `try..catch` para lidar com dados incorretos. Mas é possível que * outro erro inesperado * ocorra dentro do bloco `try {...}`? Como uma variável é indefinida ou outra coisa, não apenas essa coisa de "dados incorretos".

Como isso:

`` `js run
Deixe json = '{"age": 30}'; // dados incompletos

experimentar {
usuário = JSON.parse (json); // <- esqueceu de colocar "deixar" antes do usuário

// ...
} catch (err) {
alerta ("Erro JSON:" + errar); // Erro JSON: ReferenceError: o usuário não está definido
// (não o erro JSON na verdade)
}
`` `

Claro, tudo é possível! Os programadores cometem erros. Mesmo em utilidades de código aberto usadas por milhões por décadas - de repente, um erro louco pode ser descoberto, o que leva a hacks terríveis (como aconteceu com a ferramenta `ssh`).

No nosso caso, `try..catch` é destinado a capturar erros de" dados incorretos ". Mas, por sua natureza, `catch` obtém * todos * erros de` try`. Aqui obtém um erro inesperado, mas ainda mostra a mesma mensagem `` JSON Error ''. Isso é errado e também torna o código mais difícil de depurar.

Felizmente, podemos descobrir qual o erro que recebemos, por exemplo, do `` `` `name`:

`` `js run
experimentar {
user = {/*...*/};
} catch (e) {
*! *
alerta (e.name); // "ReferenceError" para acessar uma variável indefinida
* /! *
}
`` `

A regra é simples:

** A captura só deve processar erros que conhece e "reter" todos os outros. **

A técnica de "rethrowing" pode ser explicada com mais detalhes como:

1. Catch recebe todos os erros.
2. No bloco `catch (err) {...}`, analisamos o objeto de erro 'errar'.
2. Se não sabemos como lidar com isso, então, vamos "errar".

No código abaixo, usamos rethrowing para que `catch` manipule apenas` SyntaxError`:

`` `js run
Deixe json = '{"age": 30}'; // dados incompletos
experimentar {

Deixe o usuário = JSON.parse (json);

se (! user.name) {
lance novo SyntaxError ("Dados incompletos: sem nome");
}

*! *
blabla (); // erro inesperado
* /! *

alerta (user.name);

} catch (e) {

*! *
se (e.name == "SyntaxError") {
alerta ("Erro JSON:" + e.message);
} outro {
jogue e; // retomar (*)
}
* /! *

}
`` `

O erro que lança na linha `(*) do bloco 'catch`' entende" do `try..catch` e pode ser capturado por uma construção` try..catch` externa (se existir) ou mata o script.

Então, o bloco `catch` realmente lida com erros que sabe como lidar e" ignora "todos os outros.

O exemplo abaixo demonstra como esses erros podem ser capturados por mais um nível de `try..catch`:

`` `js run
function readData () {
Deixe json = '{"age": 30}';

experimentar {
// ...
*! *
blabla (); // erro!
* /! *
} catch (e) {
// ...
se (e.name! = 'SyntaxError') {
*! *
jogue e; // retomar (não sabe como lidar com isso)
* /! *
}
}
}

experimentar {
readData ();
} catch (e) {
*! *
alerta ("captura externa obtida:" + e); // pegou!
* /! *
}
`` `

Aqui, `readData` só sabe como lidar com` SyntaxError`, enquanto o `try..catch` externo sabe como lidar com tudo.

## try..catch..finally

Espere, isso não é tudo.

A construção `try..catch` pode ter mais uma cláusula de código:` finally`.

Se existe, ele é executado em todos os casos:

- depois de `try`, se não houveram erros,
- depois de `catch`, se houve erros.

A sintaxe estendida parece assim:

`` `js
*!*experimentar*/!* {
... tente executar o código ...
} *! * catch * /! * (e) {
... lidar com erros ...
} *!*finalmente*/!* {
... execute sempre ...
}
`` `

Tente executar este código:

`` `js run
experimentar {
alerta ('try');
se (confirmar ('Fazer um erro?')) BAD_CODE ();
} catch (e) {
alerta ('catch');
} finalmente {
alerta ('finalmente');
}
`` `

O código tem duas formas de execução:

1. Se você respondeu "Sim" para "Fazer um erro?", Então "tente -> pegar -> finalmente".
2. Se você diz "Não", então "tente -> finalmente".

A cláusula `finally` geralmente é usada quando começamos a fazer algo antes do` try..catch` e queremos finalizá-lo em qualquer caso de resultado.

Por exemplo, queremos medir o tempo que uma função de números de Fibonacci `toma de fib (n)`. Naturalmente, podemos começar a medir antes de correr e terminar depois. Mas e se houver um erro durante a chamada de função? Em particular, a implementação de `fib (n)` no código abaixo retorna um erro para números negativos ou não inteiros.

A cláusula 'finalmente' é um ótimo lugar para terminar as medidas, não importa o que.

Aqui, "finalmente" garante que o tempo será medido corretamente em ambas as situações - no caso de uma execução bem sucedida de "fib" e em caso de erro nela:

`` `js run
Deixe num = + prompt ("Digite um número inteiro positivo?", 35)

Deixe dif, resultado;

função fib (n) {
se (n <0 || Math.trunc (n)! = n) {
lança um novo erro ("Não deve ser negativo, e também um número inteiro");
}
retornar n <= 1? n: fib (n - 1) + fib (n - 2);
}

deixe start = Date.now ();

experimentar {
resultado = fib (num);
} catch (e) {
resultado = 0;
*! *
} finalmente {
diff = Date.now () - start;
}
* /! *

alerta (resultado || "ocorreu");

alerta ('execution took $ {diff} ms`);
`` `

Você pode verificar executando o código inserindo `35` em` prompt` - ele executa normalmente, 'finalmente' depois de `try`. E, em seguida, entre '-1' - haverá um erro imediato, e a execução levará `0ms`. Ambas as medições são feitas corretamente.

Em outras palavras, pode haver duas maneiras de sair de uma função: um `return` ou` throw`. A cláusula `finally` lida com os dois.


`` `smart header =" Variáveis ​​são locais dentro de `try..catch..finally`"
Observe que as variáveis ​​`result` e` diff` no código acima são declaradas * antes de * `try..catch`.

Caso contrário, se `let` fosse feito dentro do bloco` {...} `, isso só seria visível dentro dele.
`` `

`` `` smart header = "` finally` e `return`"
A cláusula finally funciona para * any * exit from `try..catch`. Isso inclui um "retorno" explícito.

No exemplo abaixo, há um `return` no` try`. Neste caso, `finally` é executado imediatamente antes do controle retornar ao código externo.

`` `js run
function func () {

experimentar {
*! *
retornar 1;
* /! *

} catch (e) {
/ * ... * /
} finalmente {
*! *
alerta ('finalmente');
* /! *
}
}

alerta (func ()); // primeiro alerta de trabalhos por fim, e depois esse
`` `
`` ``

`` `` smart header = "` try..finally` "

A construção `try..finally`, sem cláusula` catch`, também é útil. Nós o aplicamos quando não queremos lidar com erros aqui, mas queremos ter certeza de que os processos que iniciamos estão finalizados.

`` `js
function func () {
// começa a fazer algo que precisa ser concluído (como medidas)
experimentar {
// ...
} finalmente {
// complete essa coisa, mesmo que tudo pareça
}
}
`` `
No código acima, um erro dentro de `try` sempre se desfaça, porque não há` catch`. Mas `finally` funciona antes que o fluxo de execução flua para fora.
`` ``

## captura global

`` `warn header =" Environment-specific "
As informações desta seção não fazem parte do JavaScript principal.
`` `

Imaginemos que temos um erro fatal fora do `try..catch`, e o script morreu. Como um erro de programação ou algo mais terrível.

Existe uma maneira de reagir a tais ocorrências? Podemos querer registrar o erro, mostrar algo ao usuário (normalmente ele não vê mensagens de erro), etc.

Não há nenhuma na especificação, mas os ambientes geralmente fornecem, porque é realmente útil. Por exemplo, o Node.JS tem [process.on ('uncaughtException')] (https://nodejs.org/api/process.html#process_event_uncaughtexception) para isso. E no navegador podemos atribuir uma função à propriedade especial [window.onerror] (mdn: api / GlobalEventHandlers / onerror). Ele será executado em caso de erro não detectado.

A sintaxe:

`` `js
window.onerror = function (mensagem, url, linha, col, erro) {
// ...
};
`` `

`mensagem '
: Mensagem de erro.

`url`
: URL do script onde o erro ocorreu.

`line`,` col`
: Números de linha e coluna onde ocorreu erro.

`erro '
: Objeto de erro.

Por exemplo:

`` `html executar alturas de atualização não confiáveis ​​= 1
<script>
*! *
window.onerror = function (mensagem, url, linha, col, erro) {
alerta (`$ {mensagem} \ n At $ {line}: $ {col} de $ {url}`);
};
* /! *

function readData () {
badFunc (); // Ouça, alguma coisa deu errado!
}

readData ();
</ script>
`` `

O papel do manipulador global `window.onerror` geralmente não é para recuperar a execução do script - provavelmente é impossível em caso de erros de programação, mas para enviar a mensagem de erro aos desenvolvedores.

Há também serviços da Web que fornecem log de erros para tais casos, como <https://errorception.com> ou <http://www.muscula.com>.

Eles funcionam assim:

1. Registremos no serviço e obtenha um pedaço de JS (ou um URL de script) deles para inserir nas páginas.
2. Esse script JS tem uma função personalizada `window.onerror`.
3. Quando ocorre um erro, envia uma solicitação de rede sobre isso ao serviço.
4. Podemos fazer login na interface da web do serviço e ver erros.

## Resumo

A construção `try..catch` permite lidar com erros de tempo de execução. Ele literalmente permite tentar executar o código e capturar erros que podem ocorrer nele.

A sintaxe é:

`` `js
experimentar {
// execute este código
} catch (err) {
// se um erro aconteceu, então salte aqui
// err é o objeto de erro
} finalmente {
// faça, em qualquer caso, após a tentativa / captura
}
`` `

Pode não haver nenhuma seção `catch` ou não` finally`, então `try..catch` e` try..finally` também são válidos.

Os objetos de erro possuem as seguintes propriedades:

- `message` - a mensagem de erro legível por humanos.
- `name` - a string com o nome do erro (nome do construtor do erro).
- `stack` (não padrão) - a pilha no momento da criação do erro.

Também podemos gerar nossos próprios erros usando o operador `throw`. Tecnicamente, o argumento de `throw` pode ser qualquer coisa, mas geralmente é um objeto de erro que herda da classe 'Error` incorporada. Mais sobre a extensão de erros no próximo capítulo.

Rethrowing é um padrão básico de tratamento de erros: um bloco `catch` geralmente espera e sabe como lidar com o tipo de erro específico, então ele deve retomar erros que ele não conhece.

Mesmo que não tenhamos `try..catch`, a maioria dos ambientes permite configurar um manipulador de erros" global "para detectar erros que" caem ". No navegador que é `window.onerror`.
