
<img src="https://media-exp1.licdn.com/dms/image/C4D16AQFBe5q0Rr5OEg/profile-displaybackgroundimage-shrink_200_800/0/1603929596357?e=2147483647&v=beta&t=vHuFpes5cNqUMmIOi7QNzgtuR0fdnM-TaY3e6UVeJYQ">

👨‍💻 Hoje sou Analista de Desenvolvimento em Salesforce.

  <a href="https://www.instagram.com/valteir.jr/" target="_blank"><img src="https://img.shields.io/badge/-Instagram-%23E4405F?style=for-the-badge&logo=instagram&logoColor=white" target="_blank"></a>

  <a href="https://www.linkedin.com/in/valteir-junior-bb722b205/" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a> 

<a href="https://trailblazer.me/id/jsilva288/" target="_blank"><img src="https://trailhead.salesforce.com/assets/trailhead-logo-5d3354441b4d8b97f21075b65e2aea266780d45943bbb36796ac25dc7cf4adc9.svg" width=80px></a>


# Salesforce Developer Repository
## Send Email

``` 
public class NotificationHandler {
    
    public final String RECORDID;
    public final String EMAIL_SUBJECT;
    public final String EMAIL_BODY;
    public final String EMAIL_ADDRESS;
    
    public NotificationHandler(String recordid, String email_subject, String email_body, String email_address){
        RECORDID = recordid;
        EMAIL_SUBJECT = email_subject;
        EMAIL_BODY = email_body;
        EMAIL_ADDRESS = email_address;
        
        
    }
    
    public void sendEmail(){
        List<Messaging.SingleEmailMessage> lstEmail = new List<Messaging.SingleEmailMessage>();
        Messaging.SingleEmailMessage email = new Messaging.SingleEmailMessage();
        email.setToAddresses(new List<String> {email_address});
        email.setPlainTextBody(email_body);
        lstEmail.add(email);
        Messaging.sendEmail(lstEmail);

    }
    
    public void handleNotification(){
        
        Map<Id, SObject> newMap = Trigger.newMap;
        Lead record = (Lead)newMap.get(recordid);
        if(record != null){
            sendEmail();
            
        }
        
    }

}

```
## TriggerClass
```
trigger NotificationHandlerTrigger on Lead (after update) {

    NotificationHandler handler = new NotificationHandler('00Q8Z00001hpBqcUAE', 'Conta atualizada', 'A conta foi atualizada', 'junhusilva@gmail.com');
    handler.handleNotification();
}

```
