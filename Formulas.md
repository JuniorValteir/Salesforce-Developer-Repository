# Salesforce-Developer-Repository

### FORMULA FIELDS 

#### Formatting an wrongly written order from custumer. 

```
IF(LEFT({!PedidoSite}, 1) != "0" && LEFT({!PedidoSite}, 1) != "O" && LEFT({!PedidoSite}, 1) != "o", "o" & {!PedidoSite}, // If custumer enters some order without the prefix letter. Insert "o" as first character
IF(LEFT({!PedidoSite}, 1) = "0" || LEFT({!PedidoSite}, 1) = "O", "o" & RIGHT({!PedidoSite}, LEN({!PedidoSite}) - 1), {!PedidoSite})) // If custumer enters the 0 number or campital O. Replace the first character to lowercase "o"
```
#### Formatting an data. 

```
IF(MONTH({!DatadeEntrega}) < 10,
    TEXT(DAY({!DatadeEntrega})) + "/0" + TEXT(MONTH({!DatadeEntrega})) + "/" + TEXT(YEAR({!DatadeEntrega})),
    TEXT(DAY({!DatadeEntrega})) + "/" + TEXT(MONTH({!DatadeEntrega})) + "/" + TEXT(YEAR({!DatadeEntrega}))
)

```
