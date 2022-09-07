# Média dos últimos x meses

-   Fórmula
    
    ``` DAX
    VAR x = NumMeses
    3mth =
    CALCULATE (
        SUM ( Table1[sale] ),
        DATESINPERIOD ( Table1[Date], LASTDATE ( Table1[Date] ), -x, MONTH )
    ) / 3
    ```