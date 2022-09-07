
---
tag: dax, timeIntelligence, powerBI, saude, semestre 
---
# Duas aferições no mesmo ano mas em semestres diferentes

-   Fórmula
    ``` DAX
    var DataSelecionada = MAX(dCalendario[Date])
    var semestreAtual = EDATE(DataSelecionada, -6)
    var semestreAnterior = EDATE(semestreAtual, -6)
    
    var esteSemestre = SUMMARIZE(
    
    								CALCULATETABLE(VALUES(Planilha1[Código]),
    
                         	DATESBETWEEN ( dCalendario[Date], semestreAtual, DataSelecionada )), 
    															Planilha1[Código])
                         
    var TBsemestreAnterior = SUMMARIZE(
    													CALCULATETABLE(VALUES(Planilha1[Código]),
    				                     	DATESBETWEEN ( dCalendario[Date], 
    																	SemestreAnterior, semestreAtual)),  Planilha1[Código])
    
    var resultado = COUNTROWS(
    						CALCULATETABLE(
    								VALUES(Planilha1[Código]),
    										Planilha1[Código] IN esteSemestre &&
    										Planilha1[Código] IN TBsemestreAnterior))
    
    RETURN resultado
    ```