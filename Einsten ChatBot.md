
<img src="https://media-exp1.licdn.com/dms/image/C4D16AQFBe5q0Rr5OEg/profile-displaybackgroundimage-shrink_200_800/0/1603929596357?e=2147483647&v=beta&t=vHuFpes5cNqUMmIOi7QNzgtuR0fdnM-TaY3e6UVeJYQ">

👨‍💻 Hoje estou estudando Desenvolvimento Salesforce. APEX / SOQL / LWC.

  <a href="https://www.instagram.com/valteir.jr/" target="_blank"><img src="https://img.shields.io/badge/-Instagram-%23E4405F?style=for-the-badge&logo=instagram&logoColor=white" target="_blank"></a>

  <a href="https://www.linkedin.com/in/valteir-junior-bb722b205/" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a> 

<a href="https://trailblazer.me/id/jsilva288/" target="_blank"><img src="https://trailhead.salesforce.com/assets/trailhead-logo-5d3354441b4d8b97f21075b65e2aea266780d45943bbb36796ac25dc7cf4adc9.svg" width=80px></a>


# Salesforce-Developer-Repository

## Apex Class using @InvocableVariable and @InvocableMethod notation to use in Einstein ChatBot. 

### Apex Class(@InvableMethod)
```
public class B2BBotBO {
    @InvocableMethod(label='getStatusPedido')
    public static List<B2BBotResult> getStatusPedido(List<B2BBotRequest> numPedidoList) { // método recebe como parametro uma variável do tipo da classe B2BBOTRequest que recebe
        // The @InvocableVariable annotation that allows the BOT to send values.
        
       	// The method return is of type B2BBotResult which receives the @InvocableVariable annotation that
        // allows you to send values to the BOT 
        
        List<B2BBotResult> retornoList = new List<B2BBotResult>(); // Instance of an object of type B2BBOTResult that carries the attributes that receive the @InvocableVariable annotation.
        
        string idPedido = numPedidoList[0].numeroPedido; // Create an idRequest variable that receives the values of INDEX '0' from the numRequest list.
        
        Order pedido = [SELECT IdSF__c, toLabel(MotivoBloqueio__c) FROM Order WHERE IdSF__c = :idPedido LIMIT 1]; // Query that return a BlockingReason__c label field using the 'toLabel' funccion 
														  // because her return will be an Api value.
        
        if(pedido != null) {                
            B2BBotResult pedidoRetorno = new B2BBotResult();        
            pedidoRetorno.statusPedido = pedido.MotivoBloqueio__c; // The attribute variable of class B2BBotResult receives the value of the LABEL of ReasonBlock__c made by the query 'order'.
    
            retornoList .add(pedidoRetorno); // Adiciona o valor à lista o valor do objeto pedidoRetorno.
        }
        return retornoList ; // Returns a value of type List B2BBotResult that can be invoked by the BOT
    }
}
```
### Result Apex Class(@InvocableVariable)
```
public class B2BBotResult {
    @InvocableVariable(label='Records for Output' description='yourDescription' required=true)
    public String statusPedido;
}
// Invoque a String variable with InvableVariable notation to use on B2BBotBO Apex class;
```
### Request Apex Class(@InvocableVariable)
```
public class B2BBotRequest {
	@InvocableVariable(label='Records for Input' description='yourDescription' required=true)
    public String numeroPedido;
}
// Invoque a String variable with InvableVariable notation to use on B2BBotBO Apex class;
```
### ApexTest

#### Positive Test
```
@isTest
public class B2BBotBOTest {
    @isTest
    public static void getStatusPedido_PositiveTest(){
        Account acc = new Account( // Criando um registro em conta 
            Name = 'Account-1',
            Phone = '(11) 3578-0800',
            PaisOrigem__c = 'UY',
            CodigoCliente__c = '789654',
            LojaCliente__c = '12'
        );
        insert acc;
        
        Order pedido = new Order(  // Criando um regisro em Pedido(Order).
            EffectiveDate = System.today(),
            IdSF__c = 'o4145862',
            AccountId = acc.Id, //  ID da conta criada para teste. 
            Status = 'B',
            MotivoBloqueio__c = '000002' // Api do Campo MotivoBloquio__c. Razão pela qual o metodo precisa resgatar somente a LABEL do campo. 
        );   
        insert pedido;
        
        B2BBotRequest request = new B2BBotRequest(); // Instânciando objeto da classe B2BBotRequest que recebe a anotação @InvocableVariable necessária para entrada 
        // de valores proveniêntes do BOT Maria.
        
        request.numeroPedido = pedido.IdSF__c;		// Atribui o valor inicial inserido no registro pedido a variável numeroPedido do objeto request.
        
        List<B2BBotResult> resultList = B2BBotBO.getStatusPedido(new List<B2BBotRequest>{request}); // Passado o objeto request e o valor do seu atributo como parametro ao metodo.
        
        
        System.assertEquals(1, resultList.size()); // Verifica se há 1 valor atribuido a lista 'results'. A lista é proviniente do retorno do metodo 'getStatusPedido' da classe B2BBotBO
            System.assertEquals('Cliente com duplicatas em atraso', resultList[0].statusPedido); // Verifica se o retorno do método corresponde a label da picklist do campo 'MotivodoBloqueio__c. 
    }
```
#### Negative Test
```
    @isTest
    public static void getStatusPedido_NegativeExceptionTest(){
        
        Account acc = new Account( // Criando um registro em conta 
            Name = 'Account-1',
            Phone = '(11) 3578-0800',
            PaisOrigem__c = 'UY',
            CodigoCliente__c = '789654',
            LojaCliente__c = '12'
        );
        insert acc;
        
        Order pedido = new Order(  // Criando um regisro em Pedido(Order).
            EffectiveDate = System.today(),
            IdSF__c = 'o4145862',
            AccountId = acc.Id, //  ID da conta criada para teste. 
            Status = 'B',
            MotivoBloqueio__c = '000002' // Api do Campo MotivoBloquio__c. Razão pela qual o metodo precisa resgatar somente a LABEL do campo. 
        ); 
        insert pedido;
        
        B2BBotRequest request = new B2BBotRequest();
        request.numeroPedido = 'o44444'; // inserindo um valor incorreto à variavel numeroPedido, que fará parte da consulta 'pedido'do metodo 'getStatusPedido'.
        
        Exception exceptionThrown;
        
        Test.startTest();
        try{
            
            List<B2BBotResult> resultList = B2BBotBO.getStatusPedido(new List<B2BBotRequest>{request}); // passando o valor incorreto ao parametro do metodo 'getStatusPedido'.
            
        }catch(Exception e){
            exceptionThrown = e;
        }
        System.assertEquals('System.QueryException', exceptionThrown.getTypeName()); // Verifica que se ao consultar o 'MotivodoBloqueio__c' com valores incorretos, é gerado uma exceção.
        Test.stopTest();
 
    }

}
```
##### Observations
```
-- Remember to change the type of Einstein Bot to some avaiable user that can access datas from organization. Recommended that you create a Custom Bot. 
-- To call an API on outputs bot, use {!ApiName}

```

