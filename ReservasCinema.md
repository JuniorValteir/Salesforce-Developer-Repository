# Treino/Teste Portugol - Reserva Cadeira Cinema
```
programa {
  funcao inicio() {

    cadeia cinema[11][11]
    inteiro opcao, fila, cadeira

    para (inteiro linha = 1; linha < 11; linha++)
    {
      para (inteiro coluna = 1; coluna < 11; coluna++)
      {
          cinema[linha][coluna] = "🪑" 
      }
    }
    cadeia voltar = "s"
    se (voltar == "s"){
      faca{
      limpa()    
      escreva("1 - Reservar\n")
      escreva("2 - Assentos\n")
      escreva("3 - Sair\n")
      leia(opcao)
      limpa()

        escolha(opcao)
        {
          caso 1:
            escreva("Fila: ")
            leia(fila)
            escreva("Cadeira: ")
            leia(cadeira)
            se (cinema[fila][cadeira] == "🪑" ){
                cinema[fila][cadeira] = "❌"
                escreva("Deseja Voltar ao menu? ")
                leia(voltar)
                
            }senao{
              escreva("Assento já ocupado. Escolha outra posição")
            }
          pare

          caso 2:

            para (inteiro linha = 1; linha < 11; linha++)
            {
              para (inteiro coluna = 1; coluna < 11; coluna++)
              {
                escreva("    ", cinema[linha][coluna],"    ") 

              }
              escreva ("\n")
            }

            escreva("\nDeseja Voltar ao menu? ")
            leia(voltar)	
          pare

          caso contrario:
            escreva("Opção Invalida 🤔")

        }
      }enquanto(opcao != 3)
    }

  }
}
