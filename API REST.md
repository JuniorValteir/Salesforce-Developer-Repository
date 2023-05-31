<img src="https://media-exp1.licdn.com/dms/image/C4D16AQFBe5q0Rr5OEg/profile-displaybackgroundimage-shrink_200_800/0/1603929596357?e=2147483647&v=beta&t=vHuFpes5cNqUMmIOi7QNzgtuR0fdnM-TaY3e6UVeJYQ">

👨‍💻 Hoje sou Analista de Desenvolvimento em Salesforce.

  <a href="https://www.instagram.com/valteir.jr/" target="_blank"><img src="https://img.shields.io/badge/-Instagram-%23E4405F?style=for-the-badge&logo=instagram&logoColor=white" target="_blank"></a>

  <a href="https://www.linkedin.com/in/valteir-junior-bb722b205/" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a> 

<a href="https://trailblazer.me/id/jsilva288/" target="_blank"><img src="https://trailhead.salesforce.com/assets/trailhead-logo-5d3354441b4d8b97f21075b65e2aea266780d45943bbb36796ac25dc7cf4adc9.svg" width=80px></a>


# GET METHOD
```
@RestResource(urlMapping='/SalesforceGetTest/*') // usando WorkBench

global class RestEndpoint {
	
    @HttpGet
    global static Account getHighNumbEmploy(){
        
        Account itemOcc = [SELECT Id, Name, NumberOfEmployees FROM Account ORDER BY NumberOfEmployees DESC NULLS LAST LIMIT 1 ];
        return itemOcc;
    }
}

----------------------------------------------------------------------------------------------------------------------------------------------------------------------

@RestResource(urlMapping='/testRestApi/Leads/*') // usando no aplicativo ARC do Chrome

VisualForce
<apex:page >
    {!$Api.Session_ID} // buscando o API da Org/Developer para usar no Request URl do aplicativo
</apex:page>

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

```

# POST METHOD
```
global with sharing class LeadInsertWrapper {  // usando WorkBench

    public string nome;
    public string sobrenome;
    public string nome_empresa;
    public string status_doLead;
    public string email;
    
    public LeadInsertWrapper() {

    }
}
@HttpPost
    global static WrapperAccount insertAccountDetails(String Name, Integer NumberOfEmployees){
        
        Account itemAcc = new Account();
        itemAcc.Name = Name;
        itemAcc.NumberOfEmployees = NumberOfEmployees;   
        insert itemAcc;
        
        WrapperAccount itemWrap = new WrapperAccount();
        itemWrap.Nome = itemAcc.Name;
        itemWrap.NumerodeEmpregados = itemAcc.NumberOfEmployees;
        itemWrap.AccountId = itemAcc.Id;
        
        
        
        return itemWrap;  
        
    }
    global class WrapperAccount{
        global string Nome {get;set;}
        global Integer NumerodeEmpregados {get;set;}
        global string AccountId  {get;set;}
        
        global WrapperAccount(){
            this.Nome = null;
            this.NumerodeEmpregados = null;
            this.AccountId = null;
        }        
    }
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

@HttpPost
    global static void insertLead(List<LeadInsertWrapper> lstLead) // usando no aplicativo ARC do Chrome
    {
        RestResponse resultRest = RestContext.response;
        
        List<Lead> lstLeadInsert = new List<Lead>();
        
        List<String> lstEmail = new List<String>();
        
        for(LeadInsertWrapper itemLead : lstLead){
            lstEmail.add(itemLead.email);
        }
        Map<String, Lead> leadMap = new Map<String, Lead>(); // usanod email como Key em operações
        for(Lead itemLead : [SELECT Id, Email FROM Lead WHERE email = :lstEmail]){
            leadMap.put(itemLead.email, itemLead);
        }
        
        
        for(LeadInsertWrapper itemLead : lstLead){         
            Lead inserLead = new Lead();
            if( leadMap.containsKey(itemLead.email)){
                inserLead = new Lead(
                    FirstName = itemLead.nome,
                    LastName = itemLead.sobrenome,
                    Company = itemLead.nome_empresa,
                    Status = itemLead.status_doLead,
                    Email = itemLead.email,
                    id = leadMap.get(itemLead.email).id
                );
            }else{
                inserLead = new Lead(
                    FirstName = itemLead.nome,
                    LastName = itemLead.sobrenome,
                    Company = itemLead.nome_empresa,
                    Status = itemLead.status_doLead,
                    Email = itemLead.email
                );
            }
            
            lstLeadInsert.add(inserLead);
        }
        
        List<String> idsResponse = new List<String>();
        
        if(lstLeadInsert.size() > 0){ // verifica conteudo da lista 
            Database.UpsertResult[] results = Database.upsert(lstLeadInsert); // traz o valores/resultados das ações e verifica se há erro
            
            List<String> lstError = new List<String>();
            for(Integer index = 0, size = results.size(); index < size; index++){
                if(results[index].isSuccess()){ 
                    idsResponse.add(results[index].Id); // salva o id do index que teve sucesso
                }else{
                    idsResponse.add('Erro ao inserir ou atualizar um registro');
                }
            }
        }
        
        resultRest.responseBody = Blob.valueof(JSON.serializePretty(idsResponse));
        resultRest.statusCode = 200;
        
    }
 }



