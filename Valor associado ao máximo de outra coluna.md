# Valor associado ao máximo de outra coluna
``` DAX

Requisição do último documento de compra =

  
MAXX(

FILTER( dPedidosCompra,

        dPedidosCompra[Nº do documento de compras] =

MAXX(

FILTER(ALL(dPedidosCompra),

dPedidosCompra[Nº do material] = SELECTEDVALUE(DadosGeraisMaterial[Nº do material])),

[Nº do documento de compras])),

[Nº requisição de compra])

```

[Formatação de horas](Formatação%20de%20horas.md)
    

[Dependência circular](Dependência%20circular.md)