importância: 5

---

# Por que dois hamsters estão cheios?

Temos dois hamsters: `speedy` e` lazy` herdando do objeto `hamster 'geral.

Quando alimentamos um deles, o outro também está cheio. Por quê? Como corrigi-lo?

`` `js run
deixe hamster = {
estômago: [],

comer alimentos) {
this.stomach.push (comida);
}
};

deixe speedy = {
__proto__: hamster
};

deixe preguiçoso = {
__proto__: hamster
};

// Este encontrou a comida
speedyat ("maçã");
alerta (speed.stomach); // maçã

// Este também o tem, por quê? repare por favor.
alerta (preguiçoso); // maçã
`` `

