
Backticks incorporam a expressão dentro de `$ {...}` na string.

`` `js run
Deixe o nome = "Ilya";

// a expressão é um número 1
alerta (`hello $ {1}`); // olá 1

// a expressão é uma string "nome"
alerta (`hello $ {" name "}`); // nome do Olá

// a expressão é uma variável, incorpora-a
alerta (`hello $ {name}`); // hello Ilya
`` `
