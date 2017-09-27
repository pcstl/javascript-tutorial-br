
`` `js run demo
function readNumber () {
deixe num;

Faz {
num = prompt ("Digite um número, por favor?", 0);
} enquanto (! isFinite (num));

se (num === null || num === '') retornar nulo;

retorno + num;
}

alerta ('Leia: $ {readNumber ()} `);
`` `

A solução é um pouco mais intrincada que poderia ser porque precisamos lidar com linhas nulas / vazias.

Então, nós realmente aceitamos a entrada até que seja um "número regular". Tanto o `null` (cancelar) quanto a linha vazia também se encaixam nessa condição, porque na forma numérica são` 0`.

Depois que paramos, precisamos tratar a linha nula e a linha vazia especialmente (retornar nulo), porque convertê-las em um número retornaria "0".

