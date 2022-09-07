---
tag: powerBi, dax, dataModeling, dimension, starSchema
---
# dCalendario

-   Tabela
    
    ``` dax
    dCalendario = 
    
    var anoMinimo = YEAR(MIN('DadosDepósitoMaterialHistórico'[Data da última modificação]))
    var dataMinima = DATE(anoMinimo, 1, 1)
    var anoMaximo = YEAR(MAX('DadosDepósitoMaterialHistórico'[Data da última modificação]))
    var dataMaxima = DATE(anoMaximo, 12, 31)
    
    var d_Calendario = CALENDAR(dataMinima, dataMaxima)
    
    return d_Calendario
    ```
    
-   Nome do mês
    
    ```jsx
    NomeMes = FORMAT(
                DATE(1, MONTH(dCalendario[Date]), 1), "MMM")
    ```