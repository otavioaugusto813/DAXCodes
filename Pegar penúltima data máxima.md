# Pegar penúltima data máxima

-   Fórmula
    
    ```DAX
    penultimaData = 
    
    var maiorData = CALCULATE(MAX(Tabela[Data]), ALLSELECTED(), VALUES(Tabela[Produto]))
    
    RETURN
    
    CALCULATE(
    			MAX(Tabela[Data]),
    			FILTER(ALLSELECTED(Tabela),
    			Tabela[Data] < maiorData)
