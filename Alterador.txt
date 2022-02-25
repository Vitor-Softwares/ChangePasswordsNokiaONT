import time
import paramiko

# LOGIN DO SSH
username = 'COLOQUE AQUI O USUARIO LOGIN DO SSH DA OLT'
password = 'COLOQUE AQUI A SENHA DE LOGIN DO SSH DA OLT'
port = 22 # ALTERE CASO A PORTA DE ACESSO SSH NÃO SEJA ESSA

# VARIAVEIS GLOBAIS
debug = False
global nlot

# ALTERAR DE SOMENTE A SENHA
def srede():
    olts()
    print(f"\nA olt {nolt} foi selecionada! \n")
    senha = input("Digite a nova senha da 2.4G! \n")
    senha2 = input("Digite a nova senha da 5G! \n")
    slot = input("Informe o SLOT de conexão! \n")  # INFORMAÇÕES PARA IDENTIFICAR A ONT
    pon = input("Informe o PON de conexão! \n")
    ont = input("Informe a ONT de conexão! \n")

    # ----------REDE 2.4G----------
    delpass = f'DLT-HGUTR069-SPARAM::HGUTR069SPARAM-1-1-{slot}-{pon}-{ont}-5;'
    setpass = f'ENT-HGUTR069-SPARAM::HGUTR069SPARAM-1-1-{slot}-{pon}-{ont}-5::::PARAMNAME=InternetGatewayDevice.LANDevice.1.WLANConfiguration.1.PreSharedKey.1.PreSharedKey,PARAMVALUE="{senha}";'
    # ----------REDE 5G----------
    delpass2 = f"DLT-HGUTR069-SPARAM::HGUTR069SPARAM-1-1-{slot}-{pon}-{ont}-7;"
    setpass2 = f'ENT-HGUTR069-SPARAM::HGUTR069SPARAM-1-1-{slot}-{pon}-{ont}-7::::PARAMNAME=InternetGatewayDevice.LANDevice.1.WLANConfiguration.5.PreSharedKey.1.PreSharedKey,PARAMVALUE="{senha2}";'

    ssh = paramiko.SSHClient()
    ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())  # REQUISIÇÃO DE LOGIN SSH
    ssh.connect(hostname=olt, username=username, password=password, port=port)

    commands = ssh.invoke_shell()
    time.sleep(5)
    commands.send(f"{delpass} {setpass} {delpass2} {setpass2}")  # COMANDOS QUE SERÃO ENVIADOS ATÉ A OLT
    time.sleep(2)

    if debug:
        output = commands.recv(65535)
        output = output.decode("utf-8")
        print(output)
        tfinal = input('Alteração realizada com sucesso! Pressione ENTER para sair. \n')
        if tfinal == '' or tfinal != '':
            exit()
    elif not debug:
        tfinal = input('Alteração realizada com sucesso! Pressione ENTER para sair. \n')
        if tfinal == '' or tfinal != '':
            exit()

# ALTERAR NOME DA REDE
def nrede():
    olts()
    print(f"\nA olt {nolt} foi selecionada! \n")
    nome = input("Digite o nome da rede 2.4G! \n")
    nome2 = input("Digite o nome da rede 5G! \n")
    slot = input("Informe o SLOT de conexão! \n")   # INFORMAÇÕES PARA IDENTIFICAR A ONT
    pon = input("Informe o PON de conexão! \n")
    ont = input("Informe a ONT de conexão! \n")

    # ----------REDE 2.4G----------
    nomerede = f'ED-HGUTR069-SPARAM::HGUTR069SPARAM-1-1-{slot}-{pon}-{ont}-4::::PARAMVALUE="{nome}";'
    # ----------REDE 5G------------                                                                    # CONSTRUINDO OS COMANDOS
    nomerede2 = f'ED-HGUTR069-SPARAM::HGUTR069SPARAM-1-1-{slot}-{pon}-{ont}-6::::PARAMVALUE="{nome2}";'

    ssh = paramiko.SSHClient()
    ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())  # REQUISIÇÃO DE LOGIN SSH
    ssh.connect(hostname=olt, username=username, password=password, port=port)

    commands = ssh.invoke_shell()
    time.sleep(5)
    commands.send(f"{nomerede} {nomerede2}")  # COMANDOS QUE SERÃO ENVIADOS ATÉ A OLT

    if debug:
        output = commands.recv(65535)
        output = output.decode("utf-8")
        print(output)
        tfinal = input('Alteração realizada com sucesso! Pressione ENTER para sair. \n')
        if tfinal == '' or tfinal != '':
            exit()
    elif not debug:
        tfinal = input('Alteração realizada com sucesso! Pressione ENTER para sair. \n')
        if tfinal == '' or tfinal != '':
            exit()

# ALTERAR SENHA E NOME DA REDE
def snrede():
    olts()
    print(f"\nA olt {nolt} foi selecionada! \n")
    nome = input("Digite o nome da rede 2.4G! \n")
    nome2 = input("Digite o nome da rede 5G! \n")
    senha = input("Digite a nova senha da 2.4G! \n")  # INFORMAÇÕES PARA IDENTIFICAR A ONT
    senha2 = input("Digite a nova senha da 5G! \n")
    slot = input("Informe o SLOT de conexão! \n")
    pon = input("Informe o PON de conexão! \n")
    ont = input("Informe a ONT de conexão! \n")

    # ----------REDE 2.4G----------
    delpass = f'DLT-HGUTR069-SPARAM::HGUTR069SPARAM-1-1-{slot}-{pon}-{ont}-5;'
    setpass = f'ENT-HGUTR069-SPARAM::HGUTR069SPARAM-1-1-{slot}-{pon}-{ont}-5::::PARAMNAME=InternetGatewayDevice.LANDevice.1.WLANConfiguration.1.PreSharedKey.1.PreSharedKey,PARAMVALUE="{senha}";'
    # ----------REDE 5G----------
    delpass2 = f"DLT-HGUTR069-SPARAM::HGUTR069SPARAM-1-1-{slot}-{pon}-{ont}-7;"
    setpass2 = f'ENT-HGUTR069-SPARAM::HGUTR069SPARAM-1-1-{slot}-{pon}-{ont}-7::::PARAMNAME=InternetGatewayDevice.LANDevice.1.WLANConfiguration.5.PreSharedKey.1.PreSharedKey,PARAMVALUE="{senha2}";'
    # ----------REDE 2.4G----------
    nomerede = f'ED-HGUTR069-SPARAM::HGUTR069SPARAM-1-1-{slot}-{pon}-{ont}-4::::PARAMVALUE="{nome}";'  # CONSTRUINDO OS COMANDOS
    # ----------REDE 5G------------
    nomerede2 = f'ED-HGUTR069-SPARAM::HGUTR069SPARAM-1-1-{slot}-{pon}-{ont}-6::::PARAMVALUE="{nome2}";'

    ssh = paramiko.SSHClient()
    ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())  # REQUISIÇÃO DE LOGIN SSH
    ssh.connect(hostname=olt, username=username, password=password, port=port)

    commands = ssh.invoke_shell()
    time.sleep(5)
    commands.send(f"{delpass} {setpass} {delpass2} {setpass2} {nomerede} {nomerede2}")  # COMANDOS QUE SERÃO ENVIADOS ATÉ A OLT
    time.sleep(2)

    if debug:
        output = commands.recv(65535)
        output = output.decode("utf-8")
        print(output)
        tfinal = input('Alteração realizada com sucesso! Pressione ENTER para sair. \n')
        if tfinal == '' or tfinal != '':
            exit()
    elif not debug:
        tfinal = input('Alteração realizada com sucesso! Pressione ENTER para sair. \n')
        if tfinal == '' or tfinal != '':
            exit()


def olts():
    global olt
    global nolt
    print('\nSelecione a matriz! \n')
    matriz = input('1 - MATRIZ 1\n2 - MATRIZ 2\n')
    if matriz == '1':
        print('Você selecionou a MATRIZ 1\n')
        print('\nSelecione a cidade\n')
        cidadeinput = input('1 - CIDADE 1\n2 - CIDADE 2\n3 - CIDADE 3\n')
        if cidadeinput == '1':
            print('Você selecionou a CIDADE 1! ')
            print('\nSelecione uma olt! \n')
            oltinput = input('1 - OLT 1 \n2 - OLT 2 \n3 - OLT 3 \n4 - OLT 4 \n5 - OLT 5 \n')
            if oltinput == '1':
                olt = 'IP OLT'
                nolt = 'OLT 1'
            elif oltinput == '2':
                olt = 'IP OLT'
                nolt = 'OLT 2'
            elif oltinput == '3':
                olt = 'IP OLT'
                nolt = 'OLT 3'
            elif oltinput == '4':
                olt = 'IP OLT'
                nolt = 'OLT 4'
            elif oltinput == '5':
                olt = 'IP OLT'
                nolt = 'OLT 5'
            else:
                print('\nOpção Invalida! \n')
                olts()

        elif cidadeinput == '2':
            print('Você selecionou a CIDADE 2! ')
            print('\nSelecione uma olt! \n')
            oltinput = input('1 - OLT 1 \n2 - OLT 2 \n3 - OLT 3 \n4 - OLT 4 \n5 - OLT 5 \n')
            if oltinput == '1':
                olt = 'IP OLT'
                nolt = 'OLT 1'
            elif oltinput == '2':
                olt = 'IP OLT'
                nolt = 'OLT 2'
            elif oltinput == '3':
                olt = 'IP OLT'
                nolt = 'OLT 3'
            elif oltinput == '4':
                olt = 'IP OLT'
                nolt = 'OLT 4'
            elif oltinput == '5':
                olt = 'IP OLT'
                nolt = 'OLT 5'
            else:
                print('\nOpção Invalida! \n')
                olts()

        elif cidadeinput == '3':
            print('Você selecionou a CIDADE 3! ')
            print('\nSelecione uma olt! \n')
            oltinput = input('1 - OLT 1 \n2 - OLT 2 \n3 - OLT 3 \n4 - OLT 4 \n5 - OLT 5 \n')
            if oltinput == '1':
                olt = 'IP OLT'
                nolt = 'OLT 1'
            elif oltinput == '2':
                olt = 'IP OLT'
                nolt = 'OLT 2'
            elif oltinput == '3':
                olt = 'IP OLT'
                nolt = 'OLT 3'
            elif oltinput == '4':
                olt = 'IP OLT'
                nolt = 'OLT 4'
            elif oltinput == '5':
                olt = 'IP OLT'
                nolt = 'OLT 5'
            else:
                print('\nOpção Invalida! \n')
                olts()
        else:
            print('\nOpção Invalida! \n')
            olts()

    elif matriz == '2':
        print('Você selecionou a MATRIZ 2!\n')
        print('\nSelecione uma olt! \n')
        oltinput2 = input('1 - OLT 1 \n2 - OLT 2 \n3 - OLT 3 \n4 - OLT 4 \n5 - OLT 5 \n')
        if oltinput2 == '1':
            olt = 'IP OLT'
            nolt = 'OLT 1'
        elif oltinput2 == '2':
            olt = 'IP OLT'
            nolt = 'OLT 2'
        elif oltinput2 == '3':
            olt = 'IP OLT'
            nolt = 'OLT 3'
        elif oltinput2 == '4':
            olt = 'IP OLT'
            nolt = 'OLT 4'
        elif oltinput2 == '5':
            olt = 'IP OLT'
            nolt = 'OLT 5'
        else:
            print('\nOpção Invalida! \n')
            olts()
    else:
        print('\nOpção Invalida! \n')
        olts()


print("\nCreated by Vitor Daniel Verli!")
print("GitHub Profile: https://github.com/VitorDanielVerli")
print("Complete Profile: https://sitevitordanielverli.web.app\n")
print('Apenas digite as informações solicitadas, o programa faz o resto! \n\n')
print('Digite o número correspondente a opção desejada e aperte ENTER! \n')


def selecao():
    global debug
    opcoes = input(
        "1 - Alterar somente a SENHA da rede! \n2 - Alterar somente o NOME da rede! \n3 - Alterar NOME e SENHA da "
        "rede! \n")
    if opcoes == '1':
        srede()
    elif opcoes == '2':
        nrede()
    elif opcoes == '3':
        snrede()
    elif opcoes == '142536':
        debug = True
        print('\nModo Desenvolvedor ATIVADO! \n') # MODO DESENVOLVEDOR ATIVA A OPÇÃO DE VER AS INFORMAÇÕES SENDO ENVIADAS DENTRO DA OLT
        selecao()
    else:
        print("\nOpção Invalida! \n")
        selecao()


selecao()
