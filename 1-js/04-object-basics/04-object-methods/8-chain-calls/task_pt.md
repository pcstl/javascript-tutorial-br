importância: 2

---

# Chaining

Há um objeto `ladder` que permite subir e descer:

`` `js
deixa ladder = {
passo 0,
acima() {
this.step ++;
},
baixa() {
this.step--;
},
showStep: function () {// mostra o passo atual
alerta (this.step);
}
};
`` `

Agora, se precisarmos fazer várias chamadas em sequência, pode fazê-lo assim:

`` `js
ladder.up ();
ladder.up ();
ladder.down ();
ladder.showStep (); // 1
`` `

Modifique o código de `up` e` down` para tornar as chamadas encadeáveis, assim:

`` `js
ladder.up (). up (). down (). showStep (); // 1
`` `

Essa abordagem é amplamente utilizada em bibliotecas de JavaScript.
