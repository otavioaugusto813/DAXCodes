# Total Vendas utilizando tabela de Produtos com múltiplos preços

-   Fórmula
    
    ``` dax
    Total Sales w/Price Adj. = 
    SUMX(Sales,
    			Sales[Quantity] 
    					* LOOKUPVALUE('Product Pricing'[Pricing Adjustments],
    							'Product Pricing'[Product Name], RELATED('Product Pricing'[Product Name],
    							'Product Pricing'[Quarter], SELECTEDVALUE(Dates[Quarter])))
    ```