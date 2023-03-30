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
