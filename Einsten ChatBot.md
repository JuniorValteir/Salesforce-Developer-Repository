# Salesforce-Developer-Repository

## Apex Class using @InvocableVariable and @InvocableMethod notation to use in Einstein ChatBot. 

#### Apex Class(@InvableMethod)
```
public class B2BBotBO {
    @InvocableMethod(label='getStatusPedido')
    public static List<Result> getStatusPedido(List<Request> numPedido) {
                                       
        List<Result> retorno = new List<Result>();
        
        string idPedido = numPedido[0].numeroPedido;
        
        Order pedido = [SELECT IdSF__c, toLabel(MotivoBloqueio__c) FROM Order WHERE IdSF__c = :idPedido LIMIT 1];
        
        if(pedido != null) {                
            Result pedidoRetorno = new Result();        
            pedidoRetorno.statusPedido = pedido.MotivoBloqueio__c;                        
            retorno.add(pedidoRetorno);
        }
        
        return retorno;
```
#### Result Apex Class(@InvocableVariable)
```
public class B2BBotResult {
    @InvocableVariable(label='Records for Output' description='yourDescription' required=true)
    public String statusPedido;
}
// Invoque a List<String> varible with InvableVariable notation to use on B2BBotBO Apex class;
```
#### Request Apex Class(@InvocableVariable)
```
public class B2BBotRequest {
	@InvocableVariable(label='Records for Input' description='yourDescription' required=true)
    public String numeroPedido;
}
// Invoque a List<String> varible with InvableVariable notation to use on B2BBotBO Apex class;
```
