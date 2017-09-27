
Você pode observar o seguinte:

`` `js no-embellecer
função pow (x, n) // <- sem espaço entre argumentos
{// <- suporte de figura em uma linha separada
Deixe o resultado = 1; // <- sem espaços para os dois lados de =
para (vamos i = 0; i <n; i ++) {resultado * = x;} // <- sem espaços
// o conteúdo de {...} deve estar em uma nova linha
resultado de retorno;
}

Deixe x = prompt ("x?", ''), n = prompt ("n?", '') // <- tecnicamente possível,
//, mas melhor torná-lo 2 linhas, também não há espaços e;
se (n <0) // <- sem espaços dentro (n <0), e deve ser uma linha extra acima dela
{// <- suporte de figura em uma linha separada
// abaixo - uma linha longa, vale a pena dividir em 2 linhas
alerta ('Power $ {n} não é suportado, insira um número inteiro maior que zero ");
}
else // <- poderia escrevê-lo em uma única linha como "} else {"
{
alerta (pow (x, n)) // sem espaços e;
}
`` `

A variante fixa:

`` `js
função pow (x, n) {
Deixe o resultado = 1;

para (vamos i = 0; i <n; i ++) {
resultado * = x;
}

resultado de retorno;
}

Deixe x = prompt ("x?", "");
Deixe n = prompt ("n?", "");

se (n <0) {
alerta ('Power $ {n} não é suportado,
digite um número inteiro maior que zero ");
} outro {
alerta (pow (x, n));
}
`` `
