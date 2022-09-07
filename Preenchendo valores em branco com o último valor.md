``` DAX

UltimoPreço =

VAR ultimaDataValida = CALCULATE( MAX('AvaliaçãoMaterialHistórico'[Data da última modificação] ),

                            FILTER(

                                ALL('AvaliaçãoMaterialHistórico'),

                                  'AvaliaçãoMaterialHistórico'[Data da última modificação] <= MAX(dCalendario[Data]) && OR('AvaliaçãoMaterialHistórico'[Preço médio móvel unitário] <> 0,ISBLANK('AvaliaçãoMaterialHistórico'[Preço médio móvel unitário]))))

  

VAR ultimoPreco = CALCULATE(

                    sum('AvaliaçãoMaterialHistórico'[Preço médio móvel unitário]),

                    FILTER(ALL(dCalendario[Data]), dCalendario[Data] = ultimaDataValida),

                     USERELATIONSHIP('AvaliaçãoMaterialHistórico'[Data da última modificação], dCalendario[Data] ),

                      USERELATIONSHIP('AvaliaçãoMaterialHistórico'[Nº do material], DadosGeraisMaterial[Nº do material]))

  

RETURN

ultimoPreco
```








``` DAX
Valor Formatado =

VAR ultimaDataNonBlank = CALCULATE(

MAX(dCalendario[Data]),

FILTER(

ALL('AvaliaçãoMaterialHistórico'),

'AvaliaçãoMaterialHistórico'[Data da última modificação] <= MAX(dCalendario[Data]) &&

'AvaliaçãoMaterialHistórico'[Preço médio móvel unitário] <> 0

)

)


VAR ultimoPrecoNonBlank = CALCULATE(

SUM(

'AvaliaçãoMaterialHistórico'[Preço médio móvel unitário]),

FILTER(

ALL(dCalendario),

dCalendario[Data] = ultimaDataNonBlank))

RETURN

ultimoPrecoNonBlank
```
![](../../Anexos/Pasted%20image%2020220826160514.png)