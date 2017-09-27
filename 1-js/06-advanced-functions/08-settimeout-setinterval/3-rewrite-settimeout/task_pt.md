importância: 4

---

# Reescreva setTimeout com setInterval

Aqui está a função que usa 'setTimeout' aninhado para dividir um trabalho em pedaços.

Reescreva-o para `setInterval`:

`` `js run
deixe i = 0;

deixe start = Date.now ();

contagem de função () {

se (i == 1000000000) {
alerta ("Concluído em" + (Date.now () - start) + 'ms');
} outro {
setTimeout (contagem, 0);
}

// um trabalho pesado
para (let j = 0; j <1000000; j ++) {
i ++;
}

}

contagem();
`` `
