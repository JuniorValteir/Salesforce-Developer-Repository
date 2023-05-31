
<img src="https://media-exp1.licdn.com/dms/image/C4D16AQFBe5q0Rr5OEg/profile-displaybackgroundimage-shrink_200_800/0/1603929596357?e=2147483647&v=beta&t=vHuFpes5cNqUMmIOi7QNzgtuR0fdnM-TaY3e6UVeJYQ">

👨‍💻 Hoje estou estudando Desenvolvimento Salesforce. APEX / SOQL / LWC.

  <a href="https://www.instagram.com/valteir.jr/" target="_blank"><img src="https://img.shields.io/badge/-Instagram-%23E4405F?style=for-the-badge&logo=instagram&logoColor=white" target="_blank"></a>

  <a href="https://www.linkedin.com/in/valteir-junior-bb722b205/" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a> 

<a href="https://trailblazer.me/id/jsilva288/" target="_blank"><img src="https://trailhead.salesforce.com/assets/trailhead-logo-5d3354441b4d8b97f21075b65e2aea266780d45943bbb36796ac25dc7cf4adc9.svg" width=80px></a>


# Salesforce-Developer-Repository

## The method does not allow the stages of the StageName field to return
```
public class OpportunityBO {
    
    public static void cannotReturn (List <Opportunity> lstOppNew, Map <Id, Opportunity> mapOppOld){
        
        // Usando um objeto de descrição de campo e atribuindo os dados do campo StageName;
        Schema.DescribeFieldResult fieldResult = Opportunity.StageName.getDescribe();
        List<Schema.PicklistEntry> values = fieldResult.getPicklistValues(); // Lista de valores de entrada;
        List <String> lstValues = new List <String> (); // Adicionando valores a uma lista de Strings;
        for(Schema.PicklistEntry Counter: values){
            lstValues.add(Counter.getLabel());
        } 
        
        for (Opportunity Counter: lstOppNew){
            String NewStage = Counter.StageName;
            String OldStage = mapOppOld.get(Counter.id).StageName;
            Boolean NewStop = false;
            Boolean OldStop = false;
            Integer indexNew = 0;
            Integer indexOld = 0;
            
            Do{ // Localizando index da opção da Trigger.New;
                String lstCounter = lstValues[indexNew];
                if (NewStage == lstCounter){
                    NewStop = true;
                }else{
                    indexNew++;
                }
            }while(NewStop == false); 
            
            Do{ // Localizando index da opção da Trigger.OldMap;
                String lstCounter = lstValues[indexOld];
                if (OldStage == lstCounter){
                    OldStop = true;
                }else{
                    indexOld++; 
                }
            }while(OldStop == false); 
            
            if(indexNew < indexOld){ // Comparando os index;
                Counter.StageName.addError('A fase não pode retornar!');
            }
        }
    }   
    
```

## Creating an Account and linking a values from Lead. 
```
public static void createAccount(Lead lead){
        Account acc = new Account();
        acc.Name = lead.Company;
        acc.Phone = lead.Phone;
        acc.Website = lead.Website;
        
        List<Account> lstAccount = [SELECT Id, Name,Phone,Website FROM Account WHERE Name = :lead.Company];
        
        if(!lstAccount.isEmpty()){
            acc.Id = lstAccount[0].id;
            acc.Name = lstAccount[1].Name;
            acc.Phone = lstAccount[2].Phone;
            acc.Website = lstAccount[3].Website;
            update acc;
        }
        else{
            insert acc;
        }

    }
    
// TO TEST 
Lead myLead = new Lead();
myLead.Company = 'My Company';
myLead.Phone = '1234567890';
myLead.Website = 'www.mycompany.com';
myLead.Description = 'This is my company';

LeadToAccountConverter.createOrUpdateAccount(myLead);
```
## Updating an Age by BirthDate 
```
public static void contactAges(){
            List<Contact> lstContact = [SELECT Id,Age__c, Birthdate FROM Contact];
            List<Contact> contactsUpdate = new List<Contact>();
            Date currentDate = Date.today();
            
            
            for(Contact itemContact : lstContact){
                if(itemContact.Birthdate != Null){
                    integer age = itemContact.Birthdate.daysBetween(currentDate) / 365;
                    itemContact.Age__c = age;
                    contactsUpdate.add(itemContact);
                }
                
            }
            
            update contactsUpdate;
            
    
        }
```
## Using the Map to group a contact list sort by city
```
  public static Map<String, List<Contact>> groupContactsByCity() {
        Map<String, List<Contact>> contactsByCity = new Map<String, List<Contact>>();
        
        List<Contact> contacts = [SELECT Id, Name, City FROM Contact];
        
        for (Contact contact : contacts) {
            if (contact.City != null) {
                if (!contactsByCity.containsKey(contact.City)) {
                    contactsByCity.put(contact.City, new List<Contact>{contact});
                } else {
                    contactsByCity.get(contact.City).add(contact);
                }
            }
        }
        
        return contactsByCity;
    }
```
## Using DML comand to get a number of records sort by Date
```
public static List<AggregateResult> countRecordsByDate() {
        List<AggregateResult> results = [SELECT COUNT(Id) numRecords, Date__c date FROM Custom_Object__c GROUP BY Date__c];
        return results;
    }
```
