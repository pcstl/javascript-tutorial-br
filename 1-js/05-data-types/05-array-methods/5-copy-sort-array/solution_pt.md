Podemos usar `slice ()` para fazer uma cópia e executar o tipo nela:

`` `js run
função copySorted (arr) {
retornar arr.slice (). classificar ();
}

Deixe arr = ["HTML", "JavaScript", "CSS"];

*! *
Deixe ordenado = copySorted (arr);
* /! *

alerta (ordenado);
alerta (arr);
`` `

