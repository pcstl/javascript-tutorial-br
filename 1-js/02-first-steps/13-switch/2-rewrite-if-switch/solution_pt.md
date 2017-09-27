As duas primeiras verificações se transformam em dois casos. A terceira verificação é dividida em dois casos:

`` `js run
deixe a = + prompt ('a?', '');

mudar (a) {
caso 0:
alerta (0);
pausa;

caso 1:
alerta (1);
pausa;

caso 2:
caso 3:
alerta ('2,3');
*! *
pausa;
* /! *
}
`` `

Observe: o `break` na parte inferior não é necessário. Mas nós colocamos para tornar o código futuro-prova.

No futuro, há uma chance de querermos adicionar mais um caso, por exemplo, o caso 4. E se nos esquecemos de adicionar um intervalo antes dele, no final do "caso 3", haverá um erro. Então, esse é um tipo de auto-seguro.
