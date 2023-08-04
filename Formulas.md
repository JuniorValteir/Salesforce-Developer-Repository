<img src="https://media-exp1.licdn.com/dms/image/C4D16AQFBe5q0Rr5OEg/profile-displaybackgroundimage-shrink_200_800/0/1603929596357?e=2147483647&v=beta&t=vHuFpes5cNqUMmIOi7QNzgtuR0fdnM-TaY3e6UVeJYQ">

👨‍💻 Hoje sou Analista de Desenvolvimento em Salesforce.

  <a href="https://www.instagram.com/valteir.jr/" target="_blank"><img src="https://img.shields.io/badge/-Instagram-%23E4405F?style=for-the-badge&logo=instagram&logoColor=white" target="_blank"></a>

  <a href="https://www.linkedin.com/in/valteir-junior-bb722b205/" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a> 

<a href="https://trailblazer.me/id/jsilva288/" target="_blank"><img src="https://trailhead.salesforce.com/assets/trailhead-logo-5d3354441b4d8b97f21075b65e2aea266780d45943bbb36796ac25dc7cf4adc9.svg" width=80px></a>

### FORMULA FIELDS 

#### Formatting an wrongly written order from custumer. 

```

IF(LEFT({!PedidoSite}, 1) != "0" &&
LEFT({!PedidoSite}, 1) != "O" &&
LEFT({!PedidoSite}, 1) != "o", "o" & {!PedidoSite}, // If custumer enters some order without the prefix letter. Insert "o" as first character

IF(LEFT({!PedidoSite}, 1)= "0" ||
LEFT({!PedidoSite}, 1) = "O",
"o" & RIGHT({!PedidoSite}, LEN({!PedidoSite}) - 1), {!PedidoSite})) //If custumer enters the 0 number or campital O. Replace the first character to lowercase "o"


```
#### Formatting an data. 

```
IF(MONTH({!DatadeEntrega}) < 10,
    TEXT(DAY({!DatadeEntrega})) + "/0" + TEXT(MONTH({!DatadeEntrega})) + "/" + TEXT(YEAR({!DatadeEntrega})),
    TEXT(DAY({!DatadeEntrega})) + "/" + TEXT(MONTH({!DatadeEntrega})) + "/" + TEXT(YEAR({!DatadeEntrega}))
)

```

#### Formula Farol
```
Tipo de dados	Fórmula	 	 
IF(
  AND(ISPICKVAL(Status__c, 'Em aberto'), DiasAberto__c <= 1),
  IMAGE("/img/samples/color_green.gif", "OK", 15,15),
    IF(
      AND(ISPICKVAL(Status__c, 'Em aberto'), DiasAberto__c = 2),
      IMAGE("/img/samples/color_yellow.gif", "Alerta", 15,15),
        IF(
          AND(ISPICKVAL(Status__c, 'Em aberto'), DiasAberto__c >= 3),
          IMAGE("/img/samples/color_red.gif", "NOK", 15,15),
            IF(
              ISPICKVAL(Status__c, 'Aprovado'),
              IMAGE("/img/samples/flag_green.gif", "APROVADO!", 15,15),
              IF(
                  ISPICKVAL(Status__c, 'Rejeitado'),
                  IMAGE("/img/samples/flag_red.gif", "REJEITADO!", 15,15),
               NULL
              )
            )
          )
        )
    )
```
