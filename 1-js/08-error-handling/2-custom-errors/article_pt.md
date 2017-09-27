# Erros personalizados, extensão de erro

Quando desenvolvemos algo, muitas vezes precisamos de nossas próprias classes de erro para refletir coisas específicas que podem dar errado em nossas tarefas. Para erros nas operações de rede, podemos precisar de `HttpError`, para operações de banco de dados` DbError`, para procurar operações `NotFoundError` e assim por diante.

Nossos erros devem suportar propriedades básicas de erro como `mensagem`, 'nome' e, de preferência, 'pilha'. Mas eles também podem ter outras propriedades próprias, e. Os objetos `HttpError` podem ter a propriedade` statusCode` com um valor como `404` ou` 403` ou `500`.

O JavaScript permite usar `throw` com qualquer argumento, então, tecnicamente, nossas classes de erro personalizadas não precisam herdar de` Error`. Mas se herdamos, torna-se possível usar `obj instanceof Error` para identificar objetos de erro. Portanto, é melhor herdar disso.

À medida que construímos nossa aplicação, nossos próprios erros formam naturalmente uma hierarquia, por exemplo, `HttpTimeoutError` pode herdar de` HttpError`, e assim por diante.

## Extending Error

Como exemplo, considere uma função `readUser (json)` que deve ler o JSON com os dados do usuário.

Aqui está um exemplo de como um `json` válido pode parecer:
`` `js
Deixe json = `{" name ":" John "," age ": 30}`;
`` `

Internamente, usaremos `JSON.parse`. Se receber "json" mal formado, ele lança `Syntax Error`.

Mas mesmo que `json` seja sintaticamente correto, isso não significa que seja um usuário válido, certo? Pode perder os dados necessários. Por exemplo, se não pode ter propriedades `name` e` age` que são essenciais para nossos usuários.

Nossa função `readUser (json)` não somente lê o JSON, mas verifique ("validar") os dados. Se não houver campos obrigatórios, ou o formato está errado, isso é um erro. E isso não é um `SyntaxError`, porque os dados são sintaticamente corretos, mas outro tipo de erro. Chamamos isso de ValidationError e criamos uma classe para isso. Um erro desse tipo também deve conter a informação sobre o campo ofensivo.

Nossa classe `ValidationError` deve herdar da classe 'Error` integrada.

Essa classe é incorporada, mas devemos ter seu código aproximado diante de nossos olhos, para entender o que estamos estendendo.

Então, você está aqui:

`` `js
// O "pseudocódigo" para a classe de erro incorporada definida pelo próprio JavaScript
classe Erro {
construtor (mensagem) {
this.message = message;
this.name = "Error"; // (nomes diferentes para diferentes classes de erro incorporadas)
this.stack = <chamadas aninhadas>; // não padrão, mas a maioria dos ambientes o suporta
}
}
`` `

Agora vamos continuar e herdar `ValidationError` dele:

`` `js não são confiáveis
*! *
classe ValidationError extends Error {
* /! *
construtor (mensagem) {
super (mensagem); // (1)
this.name = "ValidationError"; // (2)
}
}

teste de funcionamento() {
lance o novo ValidationError ("Whoops!");
}

experimentar {
teste();
} catch (err) {
alerta (err.message); // Whoops!
alerta (err.name); // Erro de validação
alerta (err.stack); // uma lista de chamadas aninhadas com números de linha para cada
}
`` `

Por favor, veja o construtor:

1. Na linha `(1)` chamamos o construtor pai. O JavaScript exige que chamemos `super` no construtor filho, então isso é obrigatório. O construtor pai define a propriedade `message`.
2. O construtor pai também define a propriedade `name` para` "Error" `, então na linha` (2) `redefini-lo para o valor certo.

Vamos tentar usá-lo em `readUser (json)`:

`` `js run
classe ValidationError extends Error {
construtor (mensagem) {
super (mensagem);
this.name = "ValidationError";
}
}

// Uso
função readUser (json) {
Deixe o usuário = JSON.parse (json);

se (! user.age) {
lança novo ValidationError ("No field: age");
}
se (! user.name) {
lance o novo ValidationError ("No field: name");
}

retornar usuário;
}

// Exemplo de trabalho com try..catch

experimentar {
deixe username = readUser ('{"age": 25}');
} catch (err) {
se (instância err de ValidationError) {
*! *
alerta ("Dados inválidos:" + error.message); // Dados inválidos: Nenhum campo: nome
* /! *
} else if (err instanceof SyntaxError) {// (*)
alerta ("Erro de sintaxe JSON:" + err.message);
} outro {
errar; // erro desconhecido, reencontre-o (**)
}
}
`` `

O bloco `try..catch` no código acima manipula tanto o` ValidationError` quanto o `SyntaxError 'integrado de` JSON.parse`.

Por favor, veja como usamos `instanceof` para verificar o tipo de erro específico na linha` (*) `.

Podemos também olhar para `err.name`, assim:

`` `js
// ...
// em vez de (err instanceof SyntaxError)
} else if (err.name == "SyntaxError") {// (*)
// ...
`` `

A versão `instanceof` é muito melhor, porque no futuro vamos estender` ValidationError`, fazer subtipos dele, como `PropertyRequiredError`. E a verificação `instanceof` continuará a funcionar para novas classes de herança. Então isso é à prova de futuro.

Também é importante que, se `catch` atende a um erro desconhecido, então o reta na linha` (**) `. O `catch` só sabe como lidar com erros de validação e sintaxe, outros tipos (devido a um erro de digitação no código ou tal) devem cair.

## Outras heranças

A classe `ValidationError` é muito genérica. Muitas coisas podem dar errado. A propriedade pode estar ausente ou pode estar em um formato errado (como um valor de seqüência de caracteres para `age`). Vamos fazer uma classe mais concreta `PropertyRequiredError`, exatamente para propriedades ausentes. Ele irá fornecer informações adicionais sobre o imóvel que está faltando.

`` `js run
classe ValidationError extends Error {
construtor (mensagem) {
super (mensagem);
this.name = "ValidationError";
}
}

*! *
classe PropertyRequiredError extends ValidationError {
construtor (propriedade) {
super ("Sem propriedade:" + propriedade);
this.name = "PropertyRequiredError";
this.property = property;
}
}
* /! *

// Uso
função readUser (json) {
Deixe o usuário = JSON.parse (json);

se (! user.age) {
lance novo PropertyRequiredError ("idade");
}
se (! user.name) {
lance novo PropertyRequiredError ("name");
}

retornar usuário;
}

// Exemplo de trabalho com try..catch

experimentar {
deixe username = readUser ('{"age": 25}');
} catch (err) {
se (instância err de ValidationError) {
*! *
alerta ("Dados inválidos:" + error.message); // Dados inválidos: Nenhuma propriedade: nome
alerta (err.name); // PropertyRequiredError
alerta (err.property); // nome
* /! *
} else if (err instanceof SyntaxError) {
alerta ("Erro de sintaxe JSON:" + err.message);
} outro {
errar; // erro desconhecido, remanescê-lo
}
}
`` `

A nova classe `PropertyRequiredError` é fácil de usar: só precisamos passar o nome da propriedade:` new PropertyRequiredError (property) `. A "mensagem" legível por humanos é gerada pelo construtor.

Observe que `this.name` no construtor` PropertyRequiredError` é novamente atribuído manualmente. Isso pode se tornar um pouco tedius - atribuir `this.name = <class name>` ao criar cada erro personalizado. Mas há uma saída. Podemos criar nossa própria classe de "erro básico" que remove esse fardo de nossos ombros usando `this.constructor.name` para` this.name` no construtor. E então herdar dela.

Vamos chamá-lo de "MyError".

Aqui está o código com `Mirror` e outras classes de erro personalizadas, simplificado:

`` `js run
classe MyError extends Error {
construtor (mensagem) {
super (mensagem);
*! *
this.name = this.constructor.name;
* /! *
}
}

classe ValidationError extends MyError {}

classe PropertyRequiredError extends ValidationError {
construtor (propriedade) {
super ("Sem propriedade:" + propriedade);
this.property = property;
}
}

// nome é correto
alerta (novo PropertyRequiredError ("field"). nome); // PropertyRequiredError
`` `

Agora, os erros personalizados são muito mais curtos, especialmente `ValidationError`, na medida em que livramos a linha` "this.name = ..." `no construtor.

## Exceções de envolvimento

O objetivo da função `readUser` no código acima é" ler os dados do usuário ", certo? Podem ocorrer diferentes tipos de erros no processo. No momento, temos `SyntaxError` e` ValidationError`, mas no futuro a função 'readUser' pode crescer: o novo código provavelmente gerará outros tipos de erros.

O código que chama `readUser` deve lidar com esses erros. Agora, ele usa múltiplos `if` no bloco` catch` para verificar diferentes tipos de erro e reter os desconhecidos. Mas se a função 'readUser` gera vários tipos de erros - então devemos nos perguntar: realmente queremos verificar todos os tipos de erro um a um em cada código que chama `readUser`?

Muitas vezes, a resposta é "Não": o código externo quer ser "um nível acima de tudo isso". Ele quer ter algum tipo de "erro de leitura de dados". Por que exatamente aconteceu - muitas vezes é irrelevante (a mensagem de erro a descreve). Ou, ainda melhor se houver uma maneira de obter detalhes de erro, mas apenas se precisarmos.

Então, vamos fazer uma nova classe `ReadError` para representar esses erros. Se um erro ocorrer dentro de `readUser`, vamos pegar lá e gerar` ReadError`. Também manteremos a referência ao erro original na propriedade `cause`. Então, o código externo só terá que verificar o "ReadError".

Aqui está o código que define `ReadError` e demonstra sua utilização em` readUser` e `try..catch`:

`` `js run
classe ReadError extends Error {
construtor (mensagem, causa) {
super (mensagem);
this.cause = causa;
this.name = 'ReadError';
}
}

classe ValidationError extends Error {/*...*/}
class PropertyRequiredError extends ValidationError {/ * ... * /}

função validateUser (usuário) {
se (! user.age) {
lance novo PropertyRequiredError ("idade");
}

se (! user.name) {
lance novo PropertyRequiredError ("name");
}
}

função readUser (json) {
deixar o usuário;

experimentar {
usuário = JSON.parse (json);
} catch (err) {
*! *
se (instância err de SyntaxError) {
lança novo ReadError ("Erro de sintaxe", err);
} outro {
errar;
}
* /! *
}

experimentar {
validateUser (usuário);
} catch (err) {
*! *
se (instância err de ValidationError) {
lança novo ReadError ("Validation Error", err);
} outro {
errar;
}
* /! *
}

}

experimentar {
readUser ('{bad json}');
} catch (e) {
se (e instanceof ReadError) {
*! *
alerta (e);
// Erro original: SyntaxError: token in inesperado b em JSON na posição 1
alerta ("Erro original:" + e.cause);
* /! *
} outro {
jogue e;
}
}
`` `

No código acima, `readUser` funciona exatamente como descrito - captura sintaxe e erros de validação e lança erros do` ReadError` em vez disso (os erros desconhecidos são repensados ​​como de costume).

Então, o código externo verifica `instanceof ReadError` e é isso. Não é necessário listar possíveis todos os tipos de erro.

A abordagem é chamada de "exceções de enrolamento", porque levamos "exceções de baixo nível" e "embrulhá-las" em "ReadError", que é mais resumo e mais conveniente de usar para o código de chamada. É amplamente utilizado na programação orientada a objetos.

## Resumo

- Podemos herdar de `Error` e outras classes de erro incorporadas normalmente, apenas precisamos cuidar da propriedade` name` e não se esqueça de chamar `super`.
- Na maioria das vezes, devemos usar `instanceof` para verificar erros específicos. Também funciona com herança. Mas, às vezes, temos um objeto de erro proveniente da biblioteca de terceiros e não há nenhuma maneira fácil de obter a classe. Então, a propriedade `name` pode ser usada para tais verificações.
- Exceções de envolvimento é uma técnica generalizada quando uma função lida com exceções de baixo nível e faz um objeto de nível superior para informar sobre os erros. As exceções de baixo nível às vezes se tornam propriedades desse objeto como `err.cause` nos exemplos acima, mas isso não é estritamente necessário.
