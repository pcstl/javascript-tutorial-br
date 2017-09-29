A solução, passo a passo:

`` `html run
<select id = "genres">
<option value = "rock"> Rock </ option>
<valor da opção = "blues" selecionado> Blues </ option>
</ select>

<script>
// 1)
deixe selectedOption = genres.options [select.selectedIndex];
alerta (selectedOption.value);

// 2)
deixe newOption = nova opção ("classic", "Classic");
select.append (newOption);

// 3)
newOption.selected = true;
</ script>
`` `
