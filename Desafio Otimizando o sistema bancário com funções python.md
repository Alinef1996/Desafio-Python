import textwrap



def menu():
    menu = """\n
        ==================== MENU ====================

        [0]\tDepositar
        [1]\tSacar
        [2]\tExtrato
        [3]\tNovo_Cliente
        [4]\tNova_Conta
        [5]\tListar_Contas
        [6]\tSair        
        
        => """
    return input(textwrap.dedent(menu))

def depositar(saldo, valor, extrato, /):
    if valor > 0:
        saldo += valor
        extrato += f"depósito:\tR$ {valor: .2f}\n"
        print("\n=== Depósito realizado com sucesso! ===") 
    else:
        print("Desculpe! Sua operação não pode ser concluída. O valor informado é inválido.")

    return saldo, extrato

def sacar(*, saldo, valor, extrato, limite, numero_saques, limite_saques):
    excedeu_saldo = valor > saldo
    excedeu_limite = valor > limite
    excedeu_saques = numero_saques >= limite_saques

    if excedeu_saldo:
        print("\n### Não foi possivel concluir o saque. Seu saldo é insuficiente. ###")
    
    elif excedeu_limite:
        print("\n### Não foi possivel concluir o saque. O valor do saque excede o limite. ###")

    elif excedeu_saques:
        print("\n### Não foi possivel concluir o saque. O número máximo de saques foi excedido. ###")

    elif valor > 0:
        saldo -= valor
        extrato += f"Saque:\t\tR$ {valor: .2f}\n"
        numero_saques += 1
        print("\n=== Saque realizado com sucesso! ===")

    else:
        print("\n### Desculpe! Sua operação não pode ser concluída. O valor informado é inválido. ###")

    return saldo, extrato

def exibir_extrato(saldo, /, *, extrato):
    print("\n==================== EXTRATO ====================")
    print("Desculpe! Não foram realizadas movimentações." if not extrato else extrato)
    print(f"\nSaldo:\t\tR$ {saldo: .2f}")
    print("===================================================")

def cadastrar_novo_cliente(clientes):
    cpf = input("Por favor Informe o CPF (somente números): ")
    cliente = filtrar_cliente (cpf, clientes)

    if cliente:
        print("\n### Já existe um cliente cadastrado com esse CPF! ###")
        return

    nome = input("Por favor informe o nome completo: ")
    data_nascimento = input("Por favor informe a data de nascimento (dd/mm/aaaa): ")
    endereco = input("Por favor informe o endereço (logrado, número - bairro - Cidade/Estado): ")
    cep = input("Por favor informe o Cep do endereço: ")

    clientes.append({"nome": nome, "data_nascimento": data_nascimento, "cpf": cpf, "endereco": endereco, "cep": cep})

    print("=== Cliente cadastrado com sucesso! ===")

def filtrar_cliente(cpf, clientes):
    clientes_filtrados = [cliente for cliente in clientes if cliente["cpf"] == cpf]
    return clientes_filtrados[0] if clientes_filtrados else None

def criar_nova_conta(agencia, numero_conta, clientes):
    cpf = input("Por favor informe o cpf do cliente (somente números): ")
    cliente = filtrar_cliente(cpf, clientes)

    if cliente:
        print("\n=== Conta criada com sucesso ===")
        return {"agencia": agencia, "numero_conta": numero_conta, "cliente": cliente}
    
    print("\n### Cliente não encontrato , fluxo de criação de conta encerrado! ###")

def listar_contas(contas):
    for conta in contas:
        linha = f"""\
            Agência:\t{conta['agencia']}
            C/C:\t\t{conta['numero_conta']}
            Titular:\t{conta['cliente']['nome']}
        """
        print("=" * 100)
        print(textwrap.dedent(linha))

def informacoes():

    saldo = 0
    limite = 500
    extrato = ""
    numero_saques = 0
    clientes = []
    contas = []

    LIMITE_SAQUES = 3
    AGENCIA = "0001"

    while True:
        opcao = menu()
    
        if opcao == "0":
            valor = float (input("Por favor informe o valor que deseja depositar: "))

            saldo, extrato = depositar(saldo, valor, extrato)
   
        elif opcao == "1":
            valor = float (input("Por favor informe o valor que deseja sacar: "))

            saldo, extrato = sacar(
                saldo = saldo,     
                valor = valor,
                extrato = extrato,
                limite = limite,
                numero_saques = numero_saques,
                limite_saques = LIMITE_SAQUES,
            )
        
        elif opcao == "2":
            exibir_extrato(saldo, extrato=extrato)
        
        elif opcao == "3":
            cadastrar_novo_cliente(clientes)
        
        elif opcao == "4":
            numero_conta = len(contas) + 1
            conta = criar_nova_conta(AGENCIA, numero_conta, clientes)

            if conta:
                contas.append(conta)

        elif opcao == "5":
            listar_contas(contas)
            
        elif opcao == "6":
            print ("Obrigado pela sua preferência!")
            break

        else:
            print ("Desculpe! Esta operação é inválida, por favor selecione novamente a operação desejada.")


informacoes()
