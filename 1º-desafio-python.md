menu = """

[0] Depositar
[1] Sacar
[2] Extrato
[3] Sair

=> """

saldo = 0
limite = 500
extrato = ""
numero_saques = 0
LIMITE_SAQUES = 3

while True:
    opcao = input(menu)
    
    if opcao == "0":
        valor = float (input("Por favor informe o valor que deseja depositar: "))

        if valor > 0:
            saldo += valor
            extrato += f"depósito: R$ {valor: .2f}\n"
            
        else:
            print("Desculpe! Sua operação não pode ser concluída. O valor informado é inválido.")
            
    elif opcao == "1":
        valor = float (input("Por favor informe o valor que deseja sacar: "))
        
        excedeu_saldo = valor > saldo

        excedeu_limite = valor > limite
        
        excedeu_saques = numero_saques >= LIMITE_SAQUES

        if excedeu_saldo:
            print ("Não foi possivel concluir o saque. Seu saldo é insuficiente.")

        elif excedeu_limite:
            print ("Não foi possivel concluir o saque. O valor do saque excede o limite.") 
        
        elif excedeu_saques:
            print ("Não foi possivel concluir o saque. O número máximo de saques foi excedido.")

        elif valor > 0:
            saldo -= valor
            extrato += f"Saque: R$ {valor: .2f}\n"
            numero_saques += 1

        else :
            print("Desculpe! Sua operação não pode ser concluída. O valor informado é inválido.")
        
    elif opcao == "2":
        print ("\n=========== Extrato ==========")
        print ("Não foram realizadas movimentações." if not extrato else extrato)
        print (f"\nSaldo: R$ {saldo: .2f}")
        print ("================================")

    elif opcao == "3":
        print ("Obrigado pela sua preferência!")
        break

    else:
        print ("Desculpe! Esta operação é inválida, por favor selecione novamente a operação desejada.")