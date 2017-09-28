# Encontre cordas citadas

Crie uma regex para encontrar strings entre aspas duplas `subject:" ... "`.

A parte importante é que as strings devem suportar o escape, da mesma forma que as strings JavaScript fazem. Por exemplo, aspas podem ser inseridas como `subject: \" `newline as` subject: \ n`, e a própria barra como `subject: \\`.

`` `js
Deixe str = "Apenas como \" aqui \ ".";
```

Para nós, é importante que uma citação escapada `subject: \" `não termine uma string.

Então, devemos olhar de uma citação para a outra ignorando as citações de escape no caminho.

Essa é a parte essencial da tarefa, caso contrário seria trivial.

Exemplos de strings para coincidir:
`` `js
.. *!*"me teste"*/!* ..
.. *! * "Diga \" Olá \ "!" * /! * ... (citações escapadas dentro)
.. *! * "\\" * /! * .. (barra dupla para dentro)
.. *! * "\\ \" "* /! * .. (barra dupla e uma citação escapada dentro)
```

Em JavaScript, precisamos dobrar as barras para passá-las diretamente na corda, assim:

`` `js run
Deixe str = '.. "testar-me" .. "Diga \\" Olá \\ "!" ... "\\\\\\" ".. ';

// a string na memória
alerta (str); // .. "teste-me" .. "Diga \" Olá \ "!" .. "\\ \" "..
```
