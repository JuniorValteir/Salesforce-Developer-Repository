# Salesforce-Developer-Repository

### FORMULA FIELDS 

#### Formatting an wrongly written order from custumer. 

```
IF(LEFT({!PedidoSite}, 1) != "0" && LEFT({!PedidoSite}, 1) != "O" && LEFT({!PedidoSite}, 1) != "o", "o" & {!PedidoSite}, // If custumer enters some order without the prefix letter. Insert "o" as first caracter
IF(LEFT({!PedidoSite}, 1) = "0" || LEFT({!PedidoSite}, 1) = "O", "o" & RIGHT({!PedidoSite}, LEN({!PedidoSite}) - 1), {!PedidoSite})) // If custumer enters the 0 number or campital O. Replace the first caracter to lowercase "o"
```
