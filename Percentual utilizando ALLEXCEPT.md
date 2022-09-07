# Percentual utilizando ALLEXCEPT

-   Fórmula
    
    Indeed, by using ALLEXCEPT we created a measure that works if and only if the report includes the _Continent_ column. If the report does not place a filter on _Continent_, the measure stops working. Doing the math, you discover that 3.32% is the percentage of France against the total sales all over the world. The filter on Europe disappeared at the denominator, despite us explicitly asking – by using ALLEXCEPT – to keep that filter.
    
    It is common for DAX newbies to forget that ALLEXCEPT, as a CALCULATE modifier, does not introduce new filters. It can only remove existing ones. If there are no filters on _Customer[Continent]_ when ALLEXCEPT is invoked, there will be no filters after ALLEXCEPT has done its job.
    
    ```DAX
    
    PercOverContinent := 
    VAR SelSales = [Sales Amount] 
    VAR ConSales = 
        CALCULATE ( 
            [Sales Amount], 
            ALLEXCEPT ( 
                'Customer', 
                'Customer'[Continent] 
            ) 
        ) 
    VAR Result = 
        DIVIDE ( SelSales, ConSales ) 
    RETURN 
        Result
    
    ```
    

[Concatenar valores](Concatenar%20valores.md)
    

[Somar topN valores](Somar%20topN%20valores.md)