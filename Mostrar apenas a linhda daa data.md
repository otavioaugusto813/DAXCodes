``` DAX


Data =

  

var dataM = CALCULATE(  

                    MAX('fEstoqueAtual'[Data Puxada]), REMOVEFILTERS())

  

var result =

MAXX(

    FILTER (

        VALUES ( dCalendario[Data] ),

            dCalendario[Data] = dataM),

                dCalendario[Data])

var result2 =

  

CALCULATE(

    MAX(dCalendario[Data]),

    FILTER(VALUES(dCalendario[Data]),

   dCalendario[Data] = dataM))

RETURN

result2
```