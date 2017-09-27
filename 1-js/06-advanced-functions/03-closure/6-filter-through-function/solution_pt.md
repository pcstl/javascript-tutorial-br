
# Filtro entre

`` `js run
Função entre (a, b) {
função de retorno (x) {
retorno x> = a && x <= b;
};
}

deixe arr = [1, 2, 3, 4, 5, 6, 7];
alerta (arr.filter (inBetween (3, 6))); // 3,4,5,6
`` `

# Filter inArray

`` `js run
função inArray (arr) {
função de retorno (x) {
retorno arr.includes (x);
};
}

deixe arr = [1, 2, 3, 4, 5, 6, 7];
alerta (arr.filter (inArray ([1, 2, 10]))); // 1,2
`` `
