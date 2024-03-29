
<img src="https://media-exp1.licdn.com/dms/image/C4D16AQFBe5q0Rr5OEg/profile-displaybackgroundimage-shrink_200_800/0/1603929596357?e=2147483647&v=beta&t=vHuFpes5cNqUMmIOi7QNzgtuR0fdnM-TaY3e6UVeJYQ">

👨‍💻 Hoje sou Analista de Desenvolvimento em Salesforce.

  <a href="https://www.instagram.com/valteir.jr/" target="_blank"><img src="https://img.shields.io/badge/-Instagram-%23E4405F?style=for-the-badge&logo=instagram&logoColor=white" target="_blank"></a>

  <a href="https://www.linkedin.com/in/valteir-junior-bb722b205/" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a> 

<a href="https://trailblazer.me/id/jsilva288/" target="_blank"><img src="https://trailhead.salesforce.com/assets/trailhead-logo-5d3354441b4d8b97f21075b65e2aea266780d45943bbb36796ac25dc7cf4adc9.svg" width=80px></a>


# XML - Gerado via recurso WSDL2 Salesforce
```
<?xml version='1.0' encoding='UTF-8'?><wsdl:definitions xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:wsdl="http://schemas.xmlsoap.org/wsdl/" xmlns:tns="http://parks.services/" xmlns:soap="http://schemas.xmlsoap.org/wsdl/soap/" xmlns:ns1="http://schemas.xmlsoap.org/soap/http" name="ParksImplService" targetNamespace="http://parks.services/">
  <wsdl:types>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:tns="http://parks.services/" elementFormDefault="unqualified" targetNamespace="http://parks.services/" version="1.0">

  <xs:element name="byCountry" type="tns:byCountry"/>

  <xs:element name="byCountryResponse" type="tns:byCountryResponse"/>

  <xs:complexType name="byCountry">
    <xs:sequence>
      <xs:element minOccurs="0" name="arg0" type="xs:string"/>
    </xs:sequence>
  </xs:complexType>

  <xs:complexType name="byCountryResponse">
    <xs:sequence>
      <xs:element maxOccurs="unbounded" minOccurs="0" name="return" type="xs:string"/>
    </xs:sequence>
  </xs:complexType>

</xs:schema>
  </wsdl:types>
  <wsdl:message name="byCountry">
    <wsdl:part element="tns:byCountry" name="parameters">
    </wsdl:part>
  </wsdl:message>
  <wsdl:message name="byCountryResponse">
    <wsdl:part element="tns:byCountryResponse" name="parameters">
    </wsdl:part>
  </wsdl:message>
  <wsdl:portType name="Parks">
    <wsdl:operation name="byCountry">
      <wsdl:input message="tns:byCountry" name="byCountry">
    </wsdl:input>
      <wsdl:output message="tns:byCountryResponse" name="byCountryResponse">
    </wsdl:output>
    </wsdl:operation>
  </wsdl:portType>
  <wsdl:binding name="ParksImplServiceSoapBinding" type="tns:Parks">
    <soap:binding style="document" transport="http://schemas.xmlsoap.org/soap/http"/>
    <wsdl:operation name="byCountry">
      <soap:operation soapAction="" style="document"/>
      <wsdl:input name="byCountry">
        <soap:body use="literal"/>
      </wsdl:input>
      <wsdl:output name="byCountryResponse">
        <soap:body use="literal"/>
      </wsdl:output>
    </wsdl:operation>
  </wsdl:binding>
  <wsdl:service name="ParksImplService">
    <wsdl:port binding="tns:ParksImplServiceSoapBinding" name="ParksImplPort">
      <soap:address location="https://th-apex-soap-service.herokuapp.com/service/parks"/>
    </wsdl:port>
  </wsdl:service>
</wsdl:definitions>
```
# ParkService Class
```


public class ParkLocator {
		public static String[] country (string theCountry){
        ParkService.ParksImplPort parks = new ParkService.ParksImplPort();
        return parks.byCountry(theCountry);
    }
}
```

# ParkServicerMock
```


@isTest
global class ParkServiceMock implements WebServiceMock{
    global void doInvoke(
        object stub, 
        object request, 
        Map<String, Object> response,
        String endPoint,
        String soapAction,
        String requestName,
        String responseNS,
        String responseName,
        String responseType){
            ParkService.byCountryResponse response_x = new ParkService.byCountryResponse();
            response_x.return_x = new List<String>{'YellowStone', 'Yosemite National Park ', 'Joshua Tree National Park '};
            response.put('response_x', response_x);
        }
}

```
# ParkServiceTest
```

@isTest
private class ParkLocatorTest {
    @isTest static void testCallout(){
        Test.setMock(WebServiceMock.class, new ParkServiceMock());
        String country = 'United States';
        List<String> result = ParkLocator.country(country);    
        List<String> parks = new List<String>{'YellowStone', 'Yosemite National Park ', 'Joshua Tree National Park '};
        System.assertEquals(parks, result);
    }

}
