importância: 5

---

# Criar uma notificação

Escreva uma função `showNotification (options)` que uma notificação: `<div class =" notification ">` com o conteúdo fornecido. A notificação deve desaparecer automaticamente após 1,5 segundos.

As opções são:

`` `js
// mostra um elemento com o texto "Olá" perto do lado direito da janela
mostrar notificação({
top: 10, // 10px a partir do topo da janela (por padrão, 0px)
direito: 10, // 10px do lado direito da janela (por padrão, 0px)
html: "Olá!", // o HTML de notificação
className: "welcome" // uma classe adicional para o div (opcional)
});
`` `

[demo src = "solução"]


Use o posicionamento CSS para mostrar o elemento em coordenadas superior / direita. O documento original possui os estilos necessários.
