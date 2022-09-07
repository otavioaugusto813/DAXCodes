# Somar topN valores

-   Fórmula
    
    ``` dax
    Top3 Valor Estoque = SUMX (
        FILTER (
            VALUES ( fFechamentoEstoque[Cód. Item] ),
            fFechamentoEstoque[Cód. Item]
                IN TOPN ( 3, VALUES ( 'fFechamentoEstoque'[Cód. Item] ), [_Valor Estoque] )
        ),
        [_Valor Estoque]
    )
    ```
[Valor associado ao máximo de outra coluna](Valor%20associado%20ao%20máximo%20de%20outra%20coluna.md)