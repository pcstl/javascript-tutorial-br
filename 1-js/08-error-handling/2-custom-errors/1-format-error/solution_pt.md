`` `js não são confiáveis
classe FormatError extends SyntaxError {
construtor (mensagem) {
super (mensagem);
this.name = "FormatError";
}
}

Deixe err = new FormatError ("erro de formatação");

alerta (err.message); // erro de formatação
alerta (err.name); // FormatError
alerta (err.stack); // pilha

alerta (instância err de SyntaxError); // verdade
`` `
