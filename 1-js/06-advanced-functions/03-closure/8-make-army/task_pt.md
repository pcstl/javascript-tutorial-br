importância: 5

---

# Exército de funções

O código a seguir cria uma série de "atiradores".

Cada função destina-se a produzir seu número. Mas algo está errado ...

`` `js run
function makeArmy () {
Deixe os atiradores = [];

deixe i = 0;
enquanto (i <10) {
deixe shooter = function () {// função do atirador
alerta (i); // deve mostrar seu número
};
shooters.push (atirador);
i ++;
}

Tiradores de retorno;
}

Deixe army = makeArmy ();

exército [0] (); // o número de atirador 0 mostra 10
exército [5] (); // e o número 5 também produz 10 ...
// ... todos os atiradores mostram 10 em vez dos 0, 1, 2, 3 ...
`` `

Por que todos os atiradores mostram o mesmo? Corrija o código para que eles funcionem como pretendido.

