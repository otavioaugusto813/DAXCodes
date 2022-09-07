# Treatas

Permite criar um relacionamento virtual entre duas tabelas em que não há relacionamento físico. No caso abaixo, calculo o preço filtrando pelo trimestre.

-   Fórmula
    
    ```DAX
    preco = CALCULATE([_preco], 
    										TREATAS(VALUES(Data[Quarter]), 'Product Pricing'[Quarter]))
