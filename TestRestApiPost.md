public class GetTempoMedioDescargaComprovei {
    
   	 @InvocableMethod(label='getTempoMedioDescarga')
	 public static List<String> obterTempoPermanenciaCliente(List<Account> accountIdList) {
        
        List<String> resultList = new List<String>();
        List<String> idList = new List<String>();
 
        String apiUrl = 'https://api.comprovei.com.br/api/1.1/util/export/documentProcessTimes';
        String username = System.Label.UserNameComprovei; // Rótulo personalizado
        String password = System.Label.PasswordComprovei; // Rótulo personalizado
       // String authHeader = 'Basic ' + EncodingUtil.base64Encode(Blob.valueOf(username + ':' + password)); // Construção do Header de chamada de Usuário e Senha
        
         
        GetTempoMedioDescargaComproveiTO.Headers headers = new GetTempoMedioDescargaComproveiTO.Headers();
        headers.username = username;
        headers.password = password;
        
         // Valida conteúdo da Lista 
         
         for(Account itemAcc : accountIdList){
             idList.add(itemAcc.Id);
             
         }
         Order order = [SELECT Id, AccountId, NumeroRota__c, PlacaVeiculo__c 
                        FROM Order 
                        WHERE AccountId IN :idList]; 
        
        Date today = System.today();
		Datetime todayDatetime = Datetime.newInstance(today.year(), today.month(), today.day());
		String todayString = todayDatetime.format('yyyy-MM-dd');
         
        GetTempoMedioDescargaComproveiTO.Filtros filtros = new GetTempoMedioDescargaComproveiTO.Filtros();
        filtros.data_inicial =  '2015-01-01';
        filtros.data_final =    todayString;
        filtros.placa_veiculo = new List<String>{'String', order.PlacaVeiculo__c};
        filtros.numero_rota = new List<String>{'int', order.NumeroRota__c};
        
        List<String> camposList = new List<String>{
           'Rota',
            'Data',
            'Região',
            'Motorista',
            'Placa',
            'Documento',
            'Código cliente',
            'CNPJ Cliente',
            'Cliente',
            'Endereço',
            'Bairro',
            'Cidade',
            'Estado',
            'CEP',
            'Latitude',
            'Longitude',
            'Início de Viagem',
            'Início Pausa',
            'Fim Pausa',
            'Chegada',
            'Início de Descarga',
            'Fim de Descarga',
            'Registro da Foto',
            'Registro da Anotação',
            'Registro de Assinatura',
            'Fim da Entrega',
            'Tempo de Permanência',
            'Tempo de viagem',
            'Tempo de viagem + permanência',
            'Sequência original',
            'Sequência realizada',
            'Embarcador',
            'Volumes',
            'Distância Percorrida',
            'Pedido',
            'Base Origem',
            'Base Destino',
            'Data Agendamento',
            'Data Últ. Ocorr.',
            'Ultima Ocorrência',
            'Status Documento',
            'Transportadora',
            'Tempo de Espera',
            'Tempo de Descarga',
            'TMA',
            'Peso Bruto',
            'Gerente',
            'Valor',
            'Veiculo',
            'Janela Min.',
            'Janela Max.',
            'Ajuste Manual',
            'Usuário Ajuste',
            'Dia da semana da entrega',
            'Chegada dentro da certa',
            'Tipo Rota',
            'Data da criação da rota',
            'Data/Hora Ultima Chegada',
            'Campo Livre 1',
            'Campo Livre 2'
            // Resto dos campos...
        };
        
        List<String> camposInclusosList = new List<String>{
            'Início de Viagem',
            'Início Pausa',
            'Fim Pausa',
            'Chegada',
            'Início de Descarga',
            'Fim de Descarga',
            'Registro da Foto',
            'Registro da Anotação',
            'Registro de Assinatura',
            'Fim da Entrega'
             
            // Resto dos campos inclusos...
        };
        
        GetTempoMedioDescargaComproveiTO.Body body = new GetTempoMedioDescargaComproveiTO.Body();
        body.formato_exportacao = 'xml';
        body.filtros = filtros;
        body.campos = camposList;
        body.campos_inclusos = camposInclusosList;
        
        GetTempoMedioDescargaComproveiTO requestDto = new GetTempoMedioDescargaComproveiTO();
        requestDto.headers = headers;
        requestDto.body = body;
        
        // Faça a chamada HTTP para a API
        Http http = new Http();
        HttpRequest request = new HttpRequest();
        request.setEndpoint(apiUrl);
        request.setMethod('POST');
        request.setHeader('Content-Type', 'application/json');
        request.setBody(JSON.serialize(requestDto));

        HttpResponse response = http.send(request);
        String responseBody = response.getBody();

        // Analise a resposta e obtenha o valor do campo "Tempo de Permanência" do cliente
        // Aqui você precisará implementar a lógica de análise do XML retornado pela API e extrair o valor desejado

        // Exemplo de implementação simplificada usando o DOM.Document para analisar XML
        Dom.Document doc = new Dom.Document();
        doc.load(responseBody);
        
        // Encontre o elemento XML que contém o valor do campo "Tempo de Permanência"
        Dom.XMLNode tempoPermanenciaNode = doc.getRootElement().getChildElement('Tempo_de_Permanencia', null);

        // Obtenha o valor do campo "Tempo de Permanência" como uma String
        String tempoPermanenciaString = tempoPermanenciaNode.getText();

        // Converta o valor para o tipo Datetime, se necessário
    	//   Datetime tempoPermanencia = Datetime.valueOf(tempoPermanenciaString);
		resultList.add(tempoPermanenciaString);
        return resultList;
    }
}
