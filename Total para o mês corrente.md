# Total para o mês corrente

-   Fórmula
    
    ``` DAX
    VAR CurrentMonth = MONTH( TODAY() )
    VAR CurrentYear = YEAR( TODAY() )
    VAR LastDay = EOMONTH( TODAY(), 0 )
    VAR FirstDay = DATE( CurrentYear, CurrentMonth, 1 )
    
    RETURN
    CALCULATE(
        [Total Sales],
        FILTER(
            Dates,
            Dates[Date] >= FirstDay &&
            Dates[Date] <= LastDay
        ),
        ALLEXCEPT( 
            Dates,
            Dates[Date]
        )
    )
    ```