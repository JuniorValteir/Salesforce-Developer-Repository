global class GetPortabilityWindows implements vlocity_cmt.VlocityOpenInterface {
    public Boolean invokeMethod(String methodName, Map<String, Object> input, Map<String, Object> outMap, Map<String, Object> options) {
        if (methodName.equals('GetWindows')) {
            GetWindows(input, outMap, options);
        }
        return true;
    }

    public void GetWindows(Map<String, Object> input, Map<String, Object> outMap, Map<String, Object> options) {
        String procedureName = 'val_CalloutPortabilityWindows';
        Map<String, Object> ipInput = new Map<String, Object> ();
        Map<String, Object> ipOptions = new Map<String, Object> ();
        Map<String, Object> ipOutput = (Map<String, Object>) vlocity_cmt.IntegrationProcedureService.runIntegrationService(procedureName, ipInput, ipOptions);

        // Verifica se o procedimento retornou uma lista válida
        if (ipOutput.containsKey('options') && ipOutput.get('options') instanceof List<Map<String, Object>>) {
            List<Map<String, Object>> windowsList = (List<Map<String, Object>>) ipOutput.get('options');

            // Crie uma lista apenas com os rótulos
            List<String> labels = new List<String>();
            for (Map<String, Object> window : windowsList) {
                if (window.containsKey('label') && window.get('label') instanceof String) {
                    labels.add((String) window.get('label'));
                }
            }

            // Defina a lista de rótulos diretamente no outMap
            outMap.put('options', labels);
        } else {
            // Caso não tenha retornado uma lista válida, você pode tomar alguma ação aqui
            // Por exemplo, lançar uma exceção ou adicionar uma mensagem ao outMap indicando o problema.
            outMap.put('errorMessage', 'O procedimento não retornou uma lista válida de windows');
        }
    }
}
