
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


-----------------------------------------------------------------consultaCEP.js----------------------------------------------------------------------------------------
  
import { LightningElement, track } from 'lwc';
import { ShowToastEvent } from 'lightning/platformShowToastEvent';
const urlViacep = 'https://viacep.com.br/ws/';

export default class ConsultaCEP extends LightningElement {

   
    @track cep;
    @track rua;
    @track bairro;
    @track cidade;
    @track uf;
    @track showResult = true;

    

    handleChangeCEP(event) {    
        this.cep = event.target.value;
    }

    handleClickCEP(event) {

              
        this.showResult = false;
        fetch(urlViacep + this.cep + '/json/', // (Promise). Getting the urlViacep and concatenate with CEP that the user input, with the format json
            {
                
            })
            .then((response) => {
                return response.json(); // returning the response in the form of JSON after the return of API data
            })
            .then((jsonResponse) => {

                let responseObj = jsonResponse;

                this.cep = responseObj['cep'];
                this.rua = responseObj['logradouro'];
                this.bairro = responseObj['bairro'];
                this.cidade = responseObj['localidade'];
                this.uf = responseObj['uf'];

                
            })
            .catch(
                error => {
                    
                window.console.log('!! 🚨 CPF INVALIDO !!' + JSON.stringify(error));
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


-----------------------------------------------------------------Informações Org--------------------------------------------------------------------------------------

Em configurações, procure por Configuração de Sites Remotos(Remote Site Settings) e clique em Novo Site Remoto Confiavel(New Remote Site)
Crie um novo site remoto com a URL desejavel. Neste caso foi usada a do site ViaCep https://viacep.com.br

Agora sua organização está pronta para receber os dados externos API's necessários. 





























