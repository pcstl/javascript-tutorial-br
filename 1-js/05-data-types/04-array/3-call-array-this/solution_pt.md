A chamada `arr [2] ()` é sintaticamente o bom velho `obj [method] ()`, no papel de `obj`, temos` arr`, e no papel de `method` temos` 2` .

Então, temos uma chamada da função `arr [2]` como um método de objeto. Naturalmente, ele recebe `this` referenciando o objeto` arr` e exibe a matriz:

`` `js run
deixe arr = ["a", "b"];

arr.push (function () {
alerta (isto);
})

arr [2] (); // "a", "b", função
`` `

A matriz possui 3 valores: inicialmente tinha dois, além da função.
