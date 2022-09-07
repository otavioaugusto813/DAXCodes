---
tags: dax, ranking, acumulado, runningTotal
type: acumulado

---
# Acumulado sem ranking

-   Fórmula 1
    
    ```DAX
    SUMX(
    
    	FILTER(
    
    		SUMMARIZE(
    	
    			ALLSELECTED(d_padrao_municipio),
    	
    				d_padrao_municipio[Município],
    	
    				"Nota Total", [Nota]),
    	
    					[Nota Total] >= CALCULATE([Nota], VALUES(d_padrao_municipio[Município]))),
    
    						[Nota Total])
    ```
    
-   Fórmula 2
    
    A variável permite acessar o contexto fora da linha.
    
    ``` DAX
    
    EVALUATE
    
    ADDCOLUMNS(
    	VALUES( Customer[Yearly Income] ),
    	"Customers", CALCULATE( COUNTROWS( Customer ) ),
    	"RT Customers", 
    		VAR CurrentYearlyIncome = Customer[Yearly Income] //---> essa variável
    		RETURN
    			COUNTROWS(
    				FILTER(
    					Customer,
    						Customer[Yearly Income] <= CurrentYearlyIncome
                  )
              )
    ORDER BY [Yearly Income]
    ```

