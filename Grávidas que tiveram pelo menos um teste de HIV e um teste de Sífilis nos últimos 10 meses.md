---
tag: dax, timeIntelligence
---
# Grávidas que tiveram pelo menos um teste de HIV e um teste de Sífilis nos últimos 10 meses

-   Fórmula
    
    ```DAX
    EVALUATE
    VAR dataSelecionada =
        MAX ( PONTEDATA[Date] )
    VAR sifilis = { "Sorologia de Sífilis", "Teste rápido de Sífilis" }
    VAR HIV = { "Sorologia de HIV", "Teste rápido de HIV" }
    VAR resultado =
        COUNTROWS (
            CALCULATETABLE (
                VALUES ( 'FT_HIV&SIFILIS'[NPF] ),
                DATESINPERIOD ( PONTEDATA[Date], DataSelecionada, -10, MONTH ),
                'FT_HIV&SIFILIS'[NPF]
                    IN CALCULATETABLE (
                        VALUES ( 'FT_HIV&SIFILIS'[NPF] ),
                'FT_HIV&SIFILIS'[DESC_PROCED_SISREDE] IN sifilis
                    )
                        && 'FT_HIV&SIFILIS'[NPF]
                            IN CALCULATETABLE (
                                VALUES ( 'FT_HIV&SIFILIS'[NPF] ),
                                'FT_HIV&SIFILIS'[DESC_PROCED_SISREDE] IN HIV
                            )
            )
        )
    RETURN
        resultado
    ```