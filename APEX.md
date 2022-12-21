
<img src="https://media-exp1.licdn.com/dms/image/C4D16AQFBe5q0Rr5OEg/profile-displaybackgroundimage-shrink_200_800/0/1603929596357?e=2147483647&v=beta&t=vHuFpes5cNqUMmIOi7QNzgtuR0fdnM-TaY3e6UVeJYQ">

👨‍💻 Hoje estou estudando Desenvolvimento Salesforce. APEX / SOQL / LWC.

  <a href="https://www.instagram.com/valteir.jr/" target="_blank"><img src="https://img.shields.io/badge/-Instagram-%23E4405F?style=for-the-badge&logo=instagram&logoColor=white" target="_blank"></a>

  <a href="https://www.linkedin.com/in/valteir-junior-bb722b205/" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a> 

<a href="https://trailblazer.me/id/jsilva288/" target="_blank"><img src="https://trailhead.salesforce.com/assets/trailhead-logo-5d3354441b4d8b97f21075b65e2aea266780d45943bbb36796ac25dc7cf4adc9.svg" width=80px></a>


# APEX (Trigger / Class Test / SOQL)

Exemplos de execução de Trigger e Classes de Testes

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
```
------------------------------------------------------------------------LeadBO--------------------------------------------------------------------------------------

public class LeadBO {
        public static void tipoEmpresa(List<Lead> leadsTrigger){
        for(Lead itemLead : leadsTrigger){
            if(itemLead.NumberOfEmployees >= 1000){
                itemLead.TamanhoEmpresa__c = 'Pequena';
            }else{
                itemLead.TamanhoEmpresa__c = 'MicroEmpresa';
            }
            
        }
    }
    public static void criaTarefa (List<Lead> lstTriggerAcc1){
        List<Task> lstTask = new List<Task>();
        for(Lead itemLead : lstTriggerAcc1){
            if(itemLead.AnnualRevenue >= 50000){
                Task newTask = new Task();
                newTask.Subject = '!! Verificar se o cadastro está completo associado ao Lead !!';
                newTask.WhoId = itemLead.Id;
                newTask.Priority = 'Normal';
                newTask.Status = 'In Progress';
                lstTask.add(newTask);
                
                
            }
            if(lstTriggerAcc1.size()>0){ //VALIDA QUE NÃO SERÁ INSERIDO LISTA DE TAREFAS VAZIA CASO ANNUALREVENUE <500000
                insert lstTask;
            }
            
        }
    }
    public static void updateRatingLead(List<Lead> lstTriggerLead){
        for(Lead itemLead : lstTriggerLead){
            if(itemLead.AnnualRevenue >=0 && itemLead.AnnualRevenue <10000 ){
                itemLead.Rating = 'Cold';
            }else if(itemLead.AnnualRevenue >=10000 && itemLead.AnnualRevenue <100000 ){
                itemLead.Rating = 'Warm';
            }else if(itemLead.AnnualRevenue >=100000){
                itemLead.Rating = 'Hot';
            }
        }
    }
    public static void preencheCampos (List<Lead> lstTriggerLead){
        List<Lead> lstLead = new List<Lead>();
        for(Lead itemLead : lstTriggerLead){
            itemLead.FirstName = 'LALA';
            itemLead.LastName = 'LELE';
           itemLead.Fax = ' (XX) XXXXX - XXXX';
           itemLead.Website = 'www.example.com';
            itemLead.AnnualRevenue = 0;
            lstLead.add(itemLead);
            
            if(lstTriggerLead.size()>0){ //VALIDA QUE NÃO SERÁ INSERIDO LISTA DE TAREFAS VAZIA
            	
            }
            
        }
        
    }
    public static void validaFax (List<Lead> lstTriggerLeads){
      
        for(Lead itemLead : lstTriggerLeads){
            if(itemLead.fax == NUll){
                itemLead.Fax = 'Sem Fax';
               
                
            }
        }
        
    }
    public static void validaWebSite(List<Lead> lstTriggerLeads){
       
        for(Lead itemLead : lstTriggerLeads){
            if(itemLead.Website == NUll){
                itemLead.Website = 'Site Indefinido';
               
            }
        }
       
    } 
    public static void validaAnnualRevenue (List<Lead> lstTriggerLeads){
       
        for(Lead itemLead : lstTriggerLeads){
            if(itemLead.AnnualRevenue == NULL){
                itemLead.AnnualRevenue = 0;
				            
                
            }
        }
       
    }
   
   
    **USING MAP**
    public static void changeDescription (List<Lead>lstLead, Map<id,Lead> mapLeadNew, Map<Id, Lead> mapLeadOld){
        
        
        for(Lead itemLead : lstLead){
            String newValue = mapLeadNew.get(itemLead.Id).Rating;
            String oldValue = mapLeadOld.get(itemLead.Id).Rating;
            
            System.debug(newValue);     
            System.debug(oldValue); 
            itemLead.Description = oldValue;
            
        }
    }
    
    
}

---------------------------------------------------------------------------TriggerLead---------------------------------------------------------------------------------



trigger TriggerLead on Lead (before insert, before update, after insert, after update) {
    
    
    if(Trigger.isBefore){
        if(Trigger.isInsert){
            LeadBO.tipoEmpresa(Trigger.new);
           LeadBo.preencheCampos(Trigger.New);
            LeadBo.criaTarefa(Trigger.new);
          	LeadBO.updateRatingLead(Trigger.new);
        }else if(Trigger.IsUpdate){
            LeadBO.tipoEmpresa(Trigger.new);
            LeadBO.updateRatingLead(Trigger.new);
            LeadBo.validaWebSite(Trigger.new);
            LeadBo.validaFax(Trigger.new);
            LeadBo.validaAnnualRevenue(Trigger.new);
         }
    }
    if(Trigger.isAfter){
        if(Trigger.isInsert){
            LeadBo.criaTarefa(Trigger.new);

        }else if(Trigger.isUpdate){
            
        }
    }
    
}

```
# TriggerLeadTest
```
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------


@isTest 
public class TriggerLeadTest {
    @isTest public static void validaTamanhoEmpresaCriarMaior1000(){
        Lead itemLead = new Lead();
        itemLead.FirstName = 'João';
        itemLead.LastName = 'PéDife';
        itemLead.Company = 'Gigant LTDA';
        itemLead.Status = 'Open - Not Contacted';
        itemLead.NumberOfEmployees = 4000;
        
        insert itemLead;
        Lead buscaLeadCriar = [SELECT Id,TamanhoEmpresa__c FROM Lead];
        System.assert(buscaLeadCriar.TamanhoEmpresa__c == 'PEQUENA' );
        
    }
    @isTest public static void validaTamanhoEmpresaCriarMenor1000(){
        Lead itemLead = new Lead();
        itemLead.FirstName = 'João';
        itemLead.LastName = 'PéDife';
        itemLead.Company = 'Gigant LTDA';
        itemLead.Status = 'Open - Not Contacted';
        itemLead.NumberOfEmployees = 800;
        
        insert itemLead;
        Lead buscaLeadCriar = [SELECT Id,TamanhoEmpresa__c FROM Lead];
        System.assert(buscaLeadCriar.TamanhoEmpresa__c == 'MicroEmpresa' );
     }
    @isTest public static void validaTamanhoEmpresaUpdateMenor1000(){
        Lead itemLead = new Lead();
        itemLead.FirstName = 'João';
        itemLead.LastName = 'PéDife';
        itemLead.Company = 'Gigant LTDA';
        itemLead.Status = 'Open - Not Contacted';
        itemLead.NumberOfEmployees = 4000;
        
        insert itemLead;
        
        itemLead.NumberOfEmployees = 800;
        update itemLead;
        Lead buscaLeadAntes = [SELECT Id,TamanhoEmpresa__c FROM Lead];
        System.assert(buscaLeadAntes.TamanhoEmpresa__c == 'MicroEmpresa' );
    }
     @isTest public static void validaTamanhoEmpresaUpdateMaior1000(){
        Lead itemLead = new Lead();
        itemLead.FirstName = 'João';
        itemLead.LastName = 'PéDife';
        itemLead.Company = 'Gigant LTDA';
        itemLead.Status = 'Open - Not Contacted';
        itemLead.NumberOfEmployees = 800;
        
        insert itemLead;
        
        itemLead.NumberOfEmployees = 1200;
        update itemLead;
        Lead buscaLeadAntes = [SELECT Id,TamanhoEmpresa__c FROM Lead];
        System.assert(buscaLeadAntes.TamanhoEmpresa__c == 'PEQUENA' );
    }
     @isTest public static void criaTarefaTest(){
        
        Lead itemLead = new Lead();
        system.debug(itemLead.AnnualRevenue);
        itemLead.FirstName = 'João';
        itemLead.LastName = 'PéDife';
        itemLead.Company = 'Gigant LTDA';
        itemLead.Status = 'Open - Not Contacted';
        itemLead.AnnualRevenue = 6000000;
        
        insert itemLead;
        
        List<Task> findTask= [SELECT Id FROM Task WHERE WHoId =: itemLead.Id];
        system.debug(findTask);
        System.assert(findTask.size()>=1);
    }
    @isTest public static void criaTarefaTest1(){
        Lead itemLead= new Lead();
        itemLead.FirstName = 'João';
        itemLead.LastName = 'PéDife';
        itemLead.Company = 'Gigant LTDA';
        itemLead.Status = 'Open - Not Contacted';
        itemLead.AnnualRevenue = 4000;
        
        insert itemLead;
        system.debug(itemLead);
        List<Task> findTask1= [SELECT Id FROM Task WHERE WHoId =: itemLead.Id];
			
            System.assert(findTask1.size()>=0);
    }
    @isTest public static void validaSubject(){
        Lead itemLead1= new Lead();
        itemLead1.FirstName = 'João';
        itemLead1.LastName = 'PéDife';
        itemLead1.Company = 'Gigant LTDA';
        itemLead1.Status = 'Open - Not Contacted';
        itemLead1.AnnualRevenue = 50002;
        insert itemLead1;
        List<Task> lstTask = [SELECT Id,Subject FROM Task];
        
        for(Task itemTask : lstTask){
            
            System.assert(itemTask.Subject == '!! Verificar se o cadastro está completo associado ao Lead !!' );
    }
}


@isTest
public class TriggerLeadTest2 {
    
	
    
    @isTest public static void validaFaxNullUpdate(){
        Lead itemLead = new Lead();
        itemLead.FirstName = 'Juaquinha';
        itemLead.LastName = 'Menina';
        itemLead.Company = 'Coca cola';
        
        
        insert itemLead;
        
        itemLead.Fax = '';
        update itemLead;
        
        List<Lead> lstLead = [SELECT Id, Fax FROM Lead ];
        for(Lead itemLead2 : lstLead){
            System.assert(itemLead2.Fax == 'Sem Fax');
        }
    }
    @isTest public static void validaWebSiteNullUpdate(){
        Lead itemLead = new Lead();
        itemLead.FirstName = 'Juaquinha';
        itemLead.LastName = 'Menina';
        itemLead.Company = 'Coca cola';
        itemLead.Website = 'www.juaquinha.com.br';
        
        insert itemLead;
        
        itemLead.Website = '';
        update itemLead;
        
        List<Lead> lstLead = [SELECT Id, WebSite FROM Lead ];
        for(Lead itemLead2 : lstLead){
            System.assert(itemLead2.Website == 'Site Indefinido');
        }
    }
    @isTest public static void validaAnnualRevenueNullUpdate(){
        Lead itemLead = new Lead();
        itemLead.FirstName = 'Juaquinha';
        itemLead.LastName = 'Menina';
        itemLead.Company = 'Coca cola';
        
        
        insert itemLead;
        
        itemLead.AnnualRevenue = Null;
        update itemLead;
        
        List<Lead> lstLead = [SELECT Id, AnnualRevenue FROM Lead ];
        for(Lead itemLead2 : lstLead){
            System.assert(itemLead2.AnnualRevenue == 0);
        }
    }
    @isTest public static void validInsertRatingCold(){
        Lead itemLead = new Lead();
        itemLead.FirstName = 'Juaquinha';
        itemLead.LastName = 'Menina';
        itemLead.Company = 'Coca cola';
        itemLead.AnnualRevenue = 10;
        insert itemLead;
        
        List<Lead> lstLead = [SELECT Id, Rating FROM Lead];
        for(Lead itemLead2: lstLead){
            System.assert(itemLead2.Rating == 'Cold');
        }
    }
    @isTest public static void validInsertRatingWarm(){
        Lead itemLead = new Lead();
        itemLead.FirstName = 'Juaquinha';
        itemLead.LastName = 'Menina';
        itemLead.Company = 'Coca cola';
        itemLead.AnnualRevenue = 11000;
        insert itemLead;
        
        List<Lead> lstLead = [SELECT Id, Rating FROM Lead];
        for(Lead itemLead2: lstLead){
            System.assert(itemLead2.Rating == 'Warm');
        }
    }
    @isTest public static void validInsertRatingHOT(){
        Lead itemLead = new Lead();
        itemLead.FirstName = 'Juaquinha';
        itemLead.LastName = 'Menina';
        itemLead.Company = 'Coca cola';
        itemLead.AnnualRevenue = 110000;
        insert itemLead;
        
        List<Lead> lstLead = [SELECT Id, Rating FROM Lead];
        for(Lead itemLead2: lstLead){
            System.assert(itemLead2.Rating == 'Hot');
        }
    }
    @isTest public static void validUpdateRatingCold(){
        Lead itemLead = new Lead();
        itemLead.FirstName = 'Juaquinha';
        itemLead.LastName = 'Menina';
        itemLead.Company = 'Coca cola';
        itemLead.AnnualRevenue = 10001;
        insert itemLead;
        itemLead.AnnualRevenue = 10;
        update itemLead;
        
        List<Lead> lstLead = [SELECT Id, Rating FROM Lead];
        for(Lead itemLead2: lstLead){
            System.assert(itemLead2.Rating == 'Cold');
        }
    }
    @isTest public static void validUpdateRatingWarm(){
        Lead itemLead = new Lead();
        itemLead.FirstName = 'Juaquinha';
        itemLead.LastName = 'Menina';
        itemLead.Company = 'Coca cola';
        itemLead.AnnualRevenue = 10;
        insert itemLead;
        itemLead.AnnualRevenue = 11000;
        update itemLead;
        
        List<Lead> lstLead = [SELECT Id, Rating FROM Lead];
        for(Lead itemLead2: lstLead){
            System.assert(itemLead2.Rating == 'Warm');
        }
    }
    @isTest public static void validUpdateRatingHOT(){
        Lead itemLead = new Lead();
        itemLead.FirstName = 'Juaquinha';
        itemLead.LastName = 'Menina';
        itemLead.Company = 'Coca cola';
        itemLead.AnnualRevenue = 11000;
        insert itemLead;
        itemLead.AnnualRevenue = 110000;
        update itemLead;
        
        List<Lead> lstLead = [SELECT Id, Rating FROM Lead];
        for(Lead itemLead2: lstLead){
            System.assert(itemLead2.Rating == 'Hot');
        }
    }
}


