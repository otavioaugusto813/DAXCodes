# Retornar o texto que possui o maior valor

-   Fórmula
    
    ```css
    
    MAXX(
        TOPN(1, 
            ADDCOLUMNS(
            VALUES(dProgramas[PROGRAMA]), "CONTAGEM", [Total Investido]), [CONTAGEM], DESC),
            [PROGRAMA])
    ```
    

[Valor total fixo](Valor%20total%20fixo.md)