# Uso da SUMX

-   Fórmula
    
    Se for usar uma medida dentro da SUMX, utilizar CALCULATE para fazer a transição de contexto de filtro para linha.
    
    ```DAX
    Valor Planejado = SUMX(fPlanejamento, [Qtd Planejada] * RELATED(dItens[valor_unitario]))
    ```