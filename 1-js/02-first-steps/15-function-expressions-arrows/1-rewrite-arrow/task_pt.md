
# Reescreva com funções de seta

Substitua Expressões de Função com funções de seta no código:

`` `js run
função perguntar (pergunta, sim, não) {
se (confirmar (pergunta)) sim ()
else no ();
}

pergunte (
"Você concorda?",
function () {alert ("Você concordou."); },
function () {alert ("Você cancelou a execução"); }
);
`` `
