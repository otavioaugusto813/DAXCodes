---
tag: dax, ultimaCompra, lastValue, earlier
---
``` DAX
Última compra = 

CALCULATE(
	MAX(PedidosCompra[Nº do documento de compras]),
	 FILTER(
		 PedidosCompra, [Nº do material] = EARLIER(PedidosCompra[Nº do material] )))

```