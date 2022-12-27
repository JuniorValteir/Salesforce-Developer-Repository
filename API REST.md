
# GET/POST 
```


@RestResource(urlMapping='/testRestApi/Leads/*')

global with sharing class LeadApi {
   
    @HttpGet
    global static void getLeads(){
        
        RestResponse resultRest = RestContext.response; // contexto de retorno/resposta da busca 
        RestRequest request = RestContext.request; // contexto de entrada/solicitação da busca 

        String firstName = request.requestURI.subString(request.requestURI.lastIndexOf('/')+1); // pegando somente o ultimo valor da URL

        List<Lead> lstLead = String.isNotBlank(firstName) ? [SELECT Id, Name, Email, Phone FROM Lead WHERE FirstName = :firstName] : 
        [SELECT Id, Name, Email, Phone FROM Lead ] ; // Lista para trazer fistName. Caso em branco, traz a lista completa de Lead

        resultRest.responseBody = Blob.valueof(JSON.serializePretty(lstLead));
        resultRest.statusCode = 200;
    }
}
