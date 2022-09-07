---
tag: concatenate, 
---
# Concatenar valores

-   Fórmula
    
    ```DAX
            CONCATENATEX (
                CALCULATETABLE ( VALUES ( 'Product'[Color] ) ),
                Product[Color],
                ", ",             -- Separator (optional)
                Product[Color],   -- Sorting expression (optional)
                ASC               -- Sorting direction (optional)
            )
    
    ```
    
    ``` DAX
    
            
        CONCATENATEX (
            CALCULATETABLE (
                SELECTCOLUMNS (
                    NATURALLEFTOUTERJOIN (
                        SELECTCOLUMNS (
                            FILTER (
                                SUMMARIZE (
                                    fConsolidadoCompras,
                                    fConsolidadoCompras[FONTE],
                                    fConsolidadoCompras[ITEM ID],
                                    fConsolidadoCompras[PROCESSO DE COMPRA]
                                ),
                                ( fConsolidadoCompras[FONTE], fConsolidadoCompras[ITEM ID], fConsolidadoCompras[PROCESSO DE COMPRA] )
                                    IN SUMMARIZE (
                                        'fEntregaDistribuição',
                                        'fEntregaDistribuição'[FONTE],
                                        'fEntregaDistribuição'[item_id],
                                        'fEntregaDistribuição'[_processoCompra]
                                    )
                            ),
                            "FONTE",
                                [FONTE] & "",
                            "item_id",
                                [ITEM ID] + 0,
                            "_processoCompra",
                                [PROCESSO DE COMPRA] & ""
                        ),
                        SELECTCOLUMNS (
                            CALCULATETABLE (
                                SUMMARIZE (
                                    'fEntregaDistribuição',
                                    'fEntregaDistribuição'[FONTE],
                                    'fEntregaDistribuição'[item_id],
                                    'fEntregaDistribuição'[_processoCompra],
                                    'fEntregaDistribuição'[UNIDADE CONTEMPLADA]
                                ),
                                NOT ISBLANK ( 'fEntregaDistribuição'[UNIDADE CONTEMPLADA] )
                            ),
                            "FONTE",
                                [FONTE] & "",
                            "item_id",
                                [item_id] + 0,
                            "_processoCompra",
                                [_processoCompra] & "",
                            "UNIDADE CONTEMPLADA", [UNIDADE CONTEMPLADA]
                        )
                    ),
                    "UNIDADE CONTEMPLADA", [UNIDADE CONTEMPLADA]
                )
            ),
            [UNIDADE CONTEMPLADA],
            ",",
            [UNIDADE CONTEMPLADA], ASC
        )
    ```