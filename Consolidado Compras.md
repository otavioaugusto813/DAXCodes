---
tag: dax, consolidado, modelagem, dataModeling, 
---
# Consolidado Compras

-   Fórmula
    
    ``` DAX
    
    VAR gcomp =
        SELECTCOLUMNS (
            ADDCOLUMNS (
                SUMMARIZE (
                    fLicitacaoIFR,
                    [item_id],
                    [_processoCompra],
                    [Fonte do Recurso]
                ),
                "STATUS GCOMP", CALCULATE(MAX ( fLicitacaoIFR[Status Processo] )),
                "QTD GCOMP", CALCULATE ( SUM ( fLicitacaoIFR[qtd_gcomp] ) ),
                "QTD IFR", CALCULATE ( SUM ( fLicitacaoIFR[qtd_ifr] ) ) + 0
            ),
            "ITEM ID", fLicitacaoIFR[item_id] + 0,
            "PROCESSO DE COMPRA", fLicitacaoIFR[_processoCompra] & "",
            "FONTE", [Fonte do Recurso] & "",
            "QTD IFR", [QTD IFR],
            "QTD GCOMP", [QTD GCOMP],
            "STATUS GCOMP", [STATUS GCOMP]
        )
    VAR contrato =
        SELECTCOLUMNS (
            ADDCOLUMNS (
                    SUMMARIZE ( fContratosGeral, [item_id], [_processoCompra] ),
                "QTD CONTRATO", CALCULATE ( SUM ( fContratosGeral[QUANTIDADE] ) ),
                "QTD AUTORIZAÇÃO PARA EMPENHO", CALCULATE(SUM(fContratosGeral[QUANTIDADE]), FILTER(VALUES(fContratosGeral[Id Status (Contrato)]), fContratosGeral[Id Status (Contrato)] = 4)),
                "STATUS CONTRATO", CALCULATE ( MAX ( fContratosGeral[Id Status (Contrato)] ) )
            ),
            "ITEM ID", fContratosGeral[item_id] + 0,
            "PROCESSO DE COMPRA", fContratosGeral[_processoCompra] & "",
            "QTD CONTRATO", [QTD CONTRATO],
            "STATUS CONTRATO", [STATUS CONTRATO]
        ) // 
    VAR empenho =
        SELECTCOLUMNS (
            // CALCULATETABLE (
                SUMMARIZE ( fEmpenho, [item_id], fEmpenho[_processoCompra], fEmpenho[FONTE] ),
             
            "QTD EMPENHO", CALCULATE ( SUM ( fEmpenho[QTD] ) ),
            "QTD EMPENHO EMITIDO", CALCULATE ( SUM ( fEmpenho[QTD] ),  FILTER(VALUES(fEmpenho[Status Processo (Empenho)]), fEmpenho[Status Processo (Empenho)] = 5)),
            "QUANTIDADE AGUARDANDO ENTREGA",
                VAR AguardandoEntrega =
                CALCULATE(
                    SUMX(
                    FILTER(
                    fEmpenho,
                    NOT
                    ( fEmpenho[item_id], fEmpenho[_processoCompra], fEmpenho[FONTE] )
                        IN SUMMARIZE ( 'fEntregaDistribuição', [item_id], [_processoCompra], [FONTE] )
                        ), SUM(fEmpenho[QTD])))
                        
                RETURN
                    AguardandoEntrega,
            "STATUS EMPENHO", CALCULATE ( MAX ( fEmpenho[Status Processo (Empenho)] ) ),
            "ITEM ID", fEmpenho[item_id] + 0,
            "PROCESSO DE COMPRA", fEmpenho[_processoCompra] & "",
            "FONTE", [FONTE] & ""
        )
    VAR entrega =
        SELECTCOLUMNS (
                SUMMARIZE (
                    'fEntregaDistribuição',
                    'fEntregaDistribuição'[item_id],
                    'fEntregaDistribuição'[_processoCompra],
                    'fEntregaDistribuição'[FONTE]
                ),
            "STATUS ENTREGA", CALCULATE ( MAX ( 'fEntregaDistribuição'[Id Status Entrega] ) ),
            "QTD EM ESTOQUE",
                CALCULATE (
                    SUM ( 'fEntregaDistribuição'[QTD] ),
                    FILTER (
                        VALUES ( 'fEntregaDistribuição'[Id Status Entrega] ),
                        [Id Status Entrega] = 7
                    )
                ),
            "QTD DISTRIBUIÇÃO",
                CALCULATE (
                    SUM ( 'fEntregaDistribuição'[QTD] ),
                    FILTER (
                        VALUES ( 'fEntregaDistribuição'[Id Status Entrega] ),
                        [Id Status Entrega] = 8
                    )
                ),
            "ITEM ID", 'fEntregaDistribuição'[item_id] + 0,
            "PROCESSO DE COMPRA", 'fEntregaDistribuição'[_processoCompra] & "",
            "FONTE", [FONTE] & ""
        )
    VAR diof =
        SELECTCOLUMNS (
                SUMMARIZE (
                    fPagamento,
                    fPagamento[item_id],
                    fPagamento[_processoCompra], 
                    fPagamento[FONTE]
                ),
            "STATUS PAGAMENTO",
                VAR statusPagamento =
                    CALCULATE ( MAX ( 'fPagamento'[Id Status] ) )
                RETURN
                    statusPagamento,
            "QTD AGUARDANDO ABERTURA PAGAMENTO",
                CALCULATE (
                    SUM ( fPagamento[QUANTIDADE PAGAMENTO] ),
                    FILTER ( dStatusPagamento, dStatusPagamento[Id] = 8 )
                ),
    
             "QTD PAGAMENTO EM CURSO",
                CALCULATE (
                    SUM ( fPagamento[QUANTIDADE PAGAMENTO] ),
                    FILTER ( dStatusPagamento, dStatusPagamento[Id] = 9 )
                ),
            "QTD PAGAMENTO CONCLUÍDO",
                CALCULATE (
                    SUM ( fPagamento[QUANTIDADE PAGAMENTO] ),
                    FILTER ( dStatusPagamento, dStatusPagamento[Id] = 10 )
                ),
            "VALOR PAGO",
                CALCULATE (
                    SUM ( fPagamento[VALOR PAGAMENTO] ),
                    FILTER ( dStatusPagamento, dStatusPagamento[Id] = 10 )
                ),
            "ITEM ID", fPagamento[item_id] + 0,
            "PROCESSO DE COMPRA", fPagamento[_processoCompra] & "",
            "FONTE", [FONTE] & ""
        )
    
    VAR gcompContrato =
        NATURALLEFTOUTERJOIN ( gcomp, contrato )
    VAR gcompContratoEmpenho =
        NATURALLEFTOUTERJOIN ( gcompContrato, empenho )
    VAR gcompContratoEmpenhoEntrega =
        NATURALLEFTOUTERJOIN ( gcompContratoEmpenho, entrega )
    VAR gcompContratoEmpenhoEntregaDiof =
        NATURALLEFTOUTERJOIN ( gcompContratoEmpenhoEntrega, diof )
    RETURN
        gcompContratoEmpenhoEntregaDiof
        // diof
    ```