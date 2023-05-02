
<img src="https://media-exp1.licdn.com/dms/image/C4D16AQFBe5q0Rr5OEg/profile-displaybackgroundimage-shrink_200_800/0/1603929596357?e=2147483647&v=beta&t=vHuFpes5cNqUMmIOi7QNzgtuR0fdnM-TaY3e6UVeJYQ">

👨‍💻 Hoje estou estudando Desenvolvimento Salesforce. APEX / SOQL / LWC.

  <a href="https://www.instagram.com/valteir.jr/" target="_blank"><img src="https://img.shields.io/badge/-Instagram-%23E4405F?style=for-the-badge&logo=instagram&logoColor=white" target="_blank"></a>

  <a href="https://www.linkedin.com/in/valteir-junior-bb722b205/" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a> 

<a href="https://trailblazer.me/id/jsilva288/" target="_blank"><img src="https://trailhead.salesforce.com/assets/trailhead-logo-5d3354441b4d8b97f21075b65e2aea266780d45943bbb36796ac25dc7cf4adc9.svg" width=80px></a>



# LWC  Consulta CEP API ViaCEP

```


-----------------------------------------------------------------consultaCEP.html--------------------------------------------------------------------------------------
<template>
    <lightning-card
      variant="Narrow"
      title="Consulta de CEP"
      icon-name="action:map"
    >
    <div class="slds-p-left_small slds-p-right_small">
        <lightning-input label="CEP" name="cep" value={cep} type="text" id="cep" max-length="9" onchange={handleChangeCEP}></lightning-input>
  
        <template if:false={showResult}>
        <lightning-input label="Rua" name="rua" value={rua} type="text" id="rua"></lightning-input>
  
        <lightning-input label="Bairro" name="bairro" value={bairro} type="text"></lightning-input>
  
        <lightning-input label="Cidade" name="cidade" value={cidade} type="text" id="cidade"></lightning-input>
  
        <lightning-input label="Estado" name="uf"  value={uf}  type="text"  id="uf"></lightning-input>
        </template>         
    </div>
      <div slot="footer">
        <lightning-button variant="brand" label="Pesquisar" onclick={handleClickCEP}></lightning-button>
      </div>
    </lightning-card>
  </template>
 ``` 

```

-----------------------------------------------------------------consultaCEP.js----------------------------------------------------------------------------------------
  
import { LightningElement, track, api } from 'lwc';
import { ShowToastEvent } from 'lightning/platformShowToastEvent';
import Enderecos__c from '@salesforce/schema/Enderecos__c';
import Cep__c from '@salesforce/schema/Enderecos__c.Cep__c';
import Account__c from '@salesforce/schema/Enderecos__c.Account__c';
import Rua__c from '@salesforce/schema/Enderecos__c.Rua__c';
import Bairro__c from '@salesforce/schema/Enderecos__c.Bairro__c';
import DDD__c from '@salesforce/schema/Enderecos__c.DDD__C';
import Cidade__c from '@salesforce/schema/Enderecos__c.Cidade__c';
import Estado__c from '@salesforce/schema/Enderecos__c.Estado__c';
import Numero__c from '@salesforce/schema/Enderecos__c.Numero__c';
import { createRecord } from 'lightning/uiRecordApi';
const urlViacep = 'https://viacep.com.br/ws/';

export default class ConsultaCEP extends LightningElement {
    @api recordId;
    @track cep;
    @track rua;
    @track bairro;
    @track cidade;
    @track estado;
    @track ddd;
    @track numero;
    @track showResult = true;

    
    endereco = {
        cep:"",
        rua: "",
        bairro: "",
        numero: "",
        cidade: "",
        estado: "",
        ddd: "",
        account:"",
        
    };

    
    
    handleChangeCEP(event) {
        this.cep = event.target.value;
        
          
        let name_ = event.target.name;
        let value_ = event.target.value;
        
        this.endereco = {...this.endereco, [name_]:value_};
        
    }
    handleChangeNumero(event) {
                
          
        let name_ = event.target.name;
        let value_ = event.target.value;
        
        this.endereco = {...this.endereco, [name_]:value_};
        
    }

    handleClickCEP(event) {

              
        this.showResult = false;
        fetch(urlViacep + this.cep+'/json/', // End point URL
            {
                
            })
            .then((response) => {
                return response.json(); // returning the response in the form of JSON
            })
            .then((jsonResponse) => {

                let responseObj = jsonResponse;

                this.cep = responseObj['cep'];
                this.endereco.cep = this.cep;
                this.rua = responseObj['logradouro'];
                this.endereco.rua = this.rua;
                this.bairro = responseObj['bairro'];
                this.endereco.bairro = this.bairro;
                this.cidade = responseObj['localidade'];
                this.endereco.cidade = this.cidade;
                this.estado = responseObj['uf'];
                this.endereco.estado = this.estado;
                this.ddd = responseObj['ddd'];
                this.endereco.ddd = this.ddd;
                
            })
            .catch(
                error => {
                console.log()    
                window.console.log('callout error ===> ' + JSON.stringify(error));
                const erro = new ShowToastEvent({
                    title:'Erro',
                    message: '!! 🚨 CPF INVALIDO !!', 
                    variant: 'error'
                });
                this.showResult = true;

                //dispara o evento da mensagem após a conclusão da promessa
                this.dispatchEvent(erro);
            })
    }
   
    createEndereco(){
        this.endereco.account = this.recordId;
        const fields = {};

        fields[Account__c.fieldApiName] = this.endereco.account;
        fields[Cep__c.fieldApiName] = this.endereco.cep;
        fields[Rua__c.fieldApiName] = this.endereco.rua;
        fields[Bairro__c.fieldApiName] = this.endereco.bairro;
        fields[DDD__c.fieldApiName] = this.endereco.ddd;
        fields[Numero__c.fieldApiName] = this.endereco.numero;
        fields[Cidade__c.fieldApiName] = this.endereco.cidade;
        fields[Estado__c.fieldApiName] = this.endereco.estado;
        
        const recordInput = {apiName: Enderecos__c.objectApiName, fields};
        createRecord(recordInput).then(
            (event)=>{
                const confirm = new ShowToastEvent({
                    title:'Endereço Criado',
                    message: 'ID do Endereço: ' + event.id,
                    variant: 'success'
                });

                
                //dispara o envento da mensagem após a conclusão da promessa
                this.dispatchEvent(confirm);
            }
        ).catch(
            (error)=>{
                const erro = new ShowToastEvent({
                    title:'Erro',
                    message: error.body.message, 
                    variant: 'error'
                });


                //dispara o evento da mensagem após a conclusão da promessa
                this.dispatchEvent(erro);
            }
        )
    }
}
    
```

```
-----------------------------------------------------------------consultaCEP.meta--------------------------------------------------------------------------------------

<LightningComponentBundle xmlns="http://soap.sforce.com/2006/04/metadata">
    <apiVersion>47.0</apiVersion>
    <isExposed>true</isExposed>
    <targets>
        <target>lightning__RecordPage</target>
        <target>lightning__AppPage</target>
        <target>lightning__HomePage</target>
    </targets>
</LightningComponentBundle>
```

```
-----------------------------------------------------------------Informações Org--------------------------------------------------------------------------------------

Em configurações, procure por Configuração de Sites Remotos(Remote Site Settings) e clique em Novo Site Remoto Confiavel(New Remote Site)
Crie um novo site remoto com a URL desejavel. Neste caso foi usada a do site ViaCep https://viacep.com.br

Agora sua organização está pronta para receber os dados externos API's necessários. 

```



























