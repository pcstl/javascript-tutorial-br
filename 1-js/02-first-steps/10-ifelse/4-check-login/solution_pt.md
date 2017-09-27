

`` `js run demo
deixe userName = prompt ("Quem está lá?", '');

se (userName == 'Admin') {

Deixe passar = prompt ('Senha?', '');

se (pass == 'TheMaster') {
alerta ('Bem-vindo!');
} else if (pass == null) {
alerta ('Cancelado');
} outro {
alerta ('senha errada');
}

} else if (userName == null) {
alerta ('Cancelado');
} outro {
alerta ("Eu não conheço você");
}
`` `

Observe os recortes verticais dentro dos blocos `if`. Eles não são tecnicamente necessários, mas tornam o código mais legível.
