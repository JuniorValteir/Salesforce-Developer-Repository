
<img src="https://media-exp1.licdn.com/dms/image/C4D16AQFBe5q0Rr5OEg/profile-displaybackgroundimage-shrink_200_800/0/1603929596357?e=2147483647&v=beta&t=vHuFpes5cNqUMmIOi7QNzgtuR0fdnM-TaY3e6UVeJYQ">

👨‍💻 Hoje estou estudando Desenvolvimento Salesforce. APEX / SOQL / LWC.

  <a href="https://www.instagram.com/valteir.jr/" target="_blank"><img src="https://img.shields.io/badge/-Instagram-%23E4405F?style=for-the-badge&logo=instagram&logoColor=white" target="_blank"></a>

  <a href="https://www.linkedin.com/in/valteir-junior-bb722b205/" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a> 

<a href="https://trailblazer.me/id/jsilva288/" target="_blank"><img src="https://trailhead.salesforce.com/assets/trailhead-logo-5d3354441b4d8b97f21075b65e2aea266780d45943bbb36796ac25dc7cf4adc9.svg" width=80px></a>


# Salesforce-Developer-Repository

## Apex Class using @InvocableVariable and @InvocableMethod notation to use in Einstein ChatBot. 

#### Apex Class(@InvableMethod)
```
public class B2BBotBO {
    @InvocableMethod(label='getStatusPedido')
    public static List<B2BBotResult> getStatusPedido(List<B2BBotRequest> numPedido) {
                                       
        List<B2BBotResult> retorno = new List<B2BBotResult>();
        
        string idPedido = numPedido[0].numeroPedido;
        
        Order pedido = [SELECT IdSF__c, toLabel(MotivoBloqueio__c) FROM Order WHERE IdSF__c = :idPedido LIMIT 1];
        
        if(pedido != null) {                
            B2BBotResult pedidoRetorno = new B2BBotResult();        
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
##### Observations
```
-- Remember to change the type of Einstein Bot to some avaiable user that can access datas from organization. Recommended that you create a Custom Bot. 
-- To call an API on outputs bot, use {!ApiName}

```
