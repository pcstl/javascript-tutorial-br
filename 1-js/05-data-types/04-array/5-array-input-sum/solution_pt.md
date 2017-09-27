Observe os detalhes sutis, mas importantes da solução. Nós não convertimos `value` para o número instantaneamente após` prompt`, porque depois de `value = + value` não poderíamos dizer uma string vazia (stop sign) do zero (número válido). Nós fazemos isso em vez disso.


`` `js run demo
função sumInput () {

Deixe números = [];

enquanto (verdadeiro) {

Deixe value = prompt ("Um número, por favor?", 0);

// devemos cancelar?
se (value === "" || value === null ||! isFinite (value)) break;

numbers.push (+ value);
}

deixe soma = 0;
para (deixe o número de números) {
soma + = número;
}
Soma de retorno;
}

alerta (sumInput ());
`` `

