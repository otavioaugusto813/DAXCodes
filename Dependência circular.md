---
tag: dax, circularDependency
---

# Dependência circular

-   Explicação
    
    Há três cenários possíveis para que isso aconteça.
    
    1.  Coluna calculada. Exemplo: duas colunas calculadas que são iguais (mesma fórmula) sendo que ambas dependem da tabela inteira. Se ambas dependem da tabela inteira, uma referencia a outra. Usar ALLEXECEPT é uma solução, já que ela permite reduzir a quantidade de dependências dos cálculos de cada coluna.
        
    2.  Entre Relacionamentos (uma virtual e outra física, por exemplo), se for entre uma coluna calculada e outra física. Se houver dependência entre valores em branco, haverá dependência circular. Se houver uma função que referencia uma linha em branco, poderá haver dependência circular. A função ALL, por exemplo, referencia a linha em branco, enquanto ALLNOBLANKROW não referencia. Filtros implícitos também referenciam, já que não são nada menos do que um FILTER(ALL[Coluna]) implícito.
        
    3.  Relacionamento entre tabelas físicas baseado em uma coluna calculada. Se a coluna calculada permitir a criação de valores em branco, podemos incorrer em dependência circular.
        
        "Because these entities might become part of a relationship sooner or later, we must avoid using any function that operates on the blank row. For example, we prefer to use **[DISTINCT](https://dax.guide/distinct/?aff=sqlbi)** instead of **[VALUES](https://dax.guide/values/?aff=sqlbi)**."
        
        Uma vez que a função CALCULATE gera uma lista ampla de dependências e que as funções ALL e VALUES levam em consideração a linha em branco, temos um conflito com outras dependências, já que não pode haver uma chave primária em branco. Se removermos a linha em branco da chave primária, podemos criar relacionamentos e colunas calculadas perfeitamente.
        
    
    -   Referências