``` dax
MêsAno do maior preço = MAXX(

TOPN(

1,

ADDCOLUMNS(

SUMMARIZE(

dCalendario, dCalendario[Ano], dCalendario[Nome Mês], dCalendario[Data]),

"@MêsAno", dCalendario[Nome Mês] & " de " & dCalendario[Ano],

"@MaiorPreço", CALCULATE( [Maior Preço])),

[@MaiorPreço], DESC, dCalendario[Data], DESC),

[@MêsAno])
```