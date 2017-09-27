Primeiro, vamos fazer HTML / CSS.

Cada componente do tempo ficaria ótimo na sua própria '<span> `:

`` `html
<div id = "clock">
<span class = "hour"> hh </ span>: <span class = "min"> mm </ span>: <span class = "sec"> ss </ span>
</ div>
`` `

Também precisamos de CSS para colorir.

A função `update` atualizará o relógio, para ser chamado por` setInterval` a cada segundo:

`` `js
atualização de função () {
let clock = document.getElementById ('clock');
*! *
Deixe data = nova Data (); // (*)
* /! *
deixe horas = date.getHours ();
se (horas <10) horas = '0' + horas;
clock.children [0] .innerHTML = horas;

deixe minutos = date.getMinutes ();
se (minutos <10) minutos = '0' + minutos;
clock.children [1] .innerHTML = minutos;

deixe segundos = date.getSeconds ();
se (segundos <10) segundos = '0' + segundos;
clock.children [2] .innerHTML = segundos;
}
`` `

Na linha `(*)` sempre verificamos a data atual. As chamadas para `setInterval` não são confiáveis: podem acontecer com atrasos.

As funções de gerenciamento de clock:

`` `js
deixe TimerId;

function clockStart () {// execute o relógio
timerId = setInterval (atualização, 1000);
atualizar(); // (*)
}

função clockStop () {
clearInterval (timerId);
timerId = null;
}
`` `

Observe que a chamada para `update ()` não está agendada apenas em `clockStart ()`, mas é executada imediatamente na linha `(*)`. Caso contrário, o visitante teria que esperar até a primeira execução do `setInterval`. E o relógio ficaria vazio até então.
