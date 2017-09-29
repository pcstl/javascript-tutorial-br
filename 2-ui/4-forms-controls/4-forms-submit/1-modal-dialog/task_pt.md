importância: 5

---

# Forma modal

Crie uma função `showPrompt (html, callback)` que mostra um formulário com a mensagem `html`, um campo de entrada e os botões 'OK / CANCEL'.

- Um usuário deve digitar algo em um campo de texto e pressionar a tecla `: Enter` ou o botão OK, então 'callback (value)` é chamado com o valor que ele inseriu.
- Caso contrário, se o usuário pressionar `key: Esc` ou CANCELAR, então é chamado callback (null).

Nos dois casos que encerra o processo de entrada e remove o formulário.

Requisitos:

- O formulário deve estar no centro da janela.
- O formulário é * modal *. Em outras palavras, nenhuma interação com o resto da página é possível até o usuário fechá-la.
- Quando o formulário é mostrado, o foco deve estar dentro do `<input>` para o usuário.
- Teclas `chave: tecla Tab` /`: Shift + Tab` deve mudar o foco entre os campos do formulário, não permita que ele seja deixado para outros elementos da página.

Exemplo de uso:

`` `js
showPrompt ("Digite algo para ... inteligente :)", função (valor) {
alerta (valor);
});
`` `

Uma demo no iframe:

[iframe src = "solução" height = 160 border = 1]

P.S. O documento de origem tem HTML / CSS para o formulário com posicionamento fixo, mas cabe a você fazer o modo.
