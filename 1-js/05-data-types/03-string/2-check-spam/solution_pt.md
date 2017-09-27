Para tornar a pesquisa insensível a maiúsculas e minúsculas, vamos trazer a string para minúsculas e depois pesquisar:

`` `js run
função checkSpam (str) {
deixe lowerStr = str.toLowerCase ();

return lowerStr.includes ('viagra') || lowerStr.includes ('xxx');
}

alerta (checkSpam ('buy ViAgRA now'));
alerta (checkSpam ('free xxxxx'));
alerta (checkSpam ("coelho inocente"));
`` `

