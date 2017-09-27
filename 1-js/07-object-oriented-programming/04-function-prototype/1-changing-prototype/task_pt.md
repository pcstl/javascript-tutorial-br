importância: 5

---

# Mudando o "protótipo"

No código abaixo, criamos `new Rabbit` e então tentemos modificar seu protótipo.

No começo, temos esse código:

`` `js run
função Rabbit () {}
Rabbit.prototype = {
come: true
};

deixe coelho = coelho novo ();

alerta (rabbit.eats); // verdade
`` `


1. Nós adicionamos mais uma corda (enfatizada), o que `alert` mostra agora?

`` `js
função Rabbit () {}
Rabbit.prototype = {
come: true
};

deixe coelho = coelho novo ();

*! *
Rabbit.prototype = {};
* /! *

alerta (rabbit.eats); //?
`` `

2. ... E se o código é assim (substituiu uma linha)?

`` `js
função Rabbit () {}
Rabbit.prototype = {
come: true
};

deixe coelho = coelho novo ();

*! *
Rabbit.prototype.eats = false;
* /! *

alerta (rabbit.eats); //?
`` `

3. Assim (substituiu uma linha)?

`` `js
função Rabbit () {}
Rabbit.prototype = {
come: true
};

deixe coelho = coelho novo ();

*! *
delete rabbit.eats;
* /! *

alerta (rabbit.eats); //?
`` `

4. A última variante:

`` `js
função Rabbit () {}
Rabbit.prototype = {
come: true
};

deixe coelho = coelho novo ();

*! *
apague Rabbit.prototype.eats;
* /! *

alerta (rabbit.eats); //?
`` `
