A solução: `pattern: /" (\\. | [^ "\\]) *" / g`.

Passo a passo:

- Primeiro procuramos um padrão de citações de abertura: "`
- Então, se tivermos um padrão de barra invertida: \\ `(nós, tecnicamente, temos que dobrá-lo no padrão, porque é um caractere especial, então é uma única barra invertida de fato), então qualquer personagem está bem depois (um ponto ).
- Caso contrário, tomamos qualquer caractere, exceto uma citação (isso significaria o fim da string) e uma barra invertida (para evitar barras invertidas solitárias, a barra invertida só é usada com algum outro símbolo depois): `padrão: [^" \\] `
- ... E assim por diante até a citação de encerramento.

Em ação:

`` `js run
Deixe reg = /"(\\.|[^"\\])*"g / g;
Deixe str = '.. "testar-me" .. "Diga \\" Olá \\ "!" ... "\\\\\\" ".. ';

alerta (str.match (reg)); // "teste-me", "Diga \" Olá \ "!", "\\ \" "
```
