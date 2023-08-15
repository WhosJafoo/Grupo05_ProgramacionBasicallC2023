# Grupo05_ProgramacionBasicallC2023
#Proyecto programación segundo cuatrimestre
import getpass
import random

# Variables globales
usuariosRegistrados = []
naipes = ['Corazones', 'Diamantes', 'Picas', 'Tréboles']
valores = {'Dos': 2, 'Tres': 3, 'Cuatro': 4, 'Cinco': 5, 'Seis': 6, 'Siete': 7,
           'Ocho': 8, 'Nueve': 9, 'Diez': 10, 'Jota': 10, 'Reina': 10, 'Rey': 10, 'As': 11}
saldoInicial = 100
apuestaMinima = 10

# Funciones del Casino
def mostrarMenu():
    print("Bienvenido a DreamWorld Casino")
    print("Seleccione una opción:")
    print("1. Registro de usuario nuevo")
    print("2. DreamWorld Casino")
    print("3. Configuración Avanzada")
    print("4. Salir")

def registrarUsuario():
    print("NUEVO USUARIO!!")
    intentos = 0
    while intentos < 3:
        idUsuario = getpass.getpass("Ingrese un nombre de usuario o ID: ")
        if len(idUsuario) < 5:
            print("El ID debe tener al menos cinco caracteres")
        elif any(usuario['idUsuario'] == idUsuario for usuario in usuariosRegistrados):
            print("El ID ingresado ya está registrado")
        else:
            nombre = input("Ingrese su nombre:\n")
            pin = getpass.getpass("Ingrese su PIN (mínimo de 6 dígitos): ")
            if len(pin) < 6:
                print("PIN inválido, mínimo seis dígitos.")
            else:
                pinConfirmacion = getpass.getpass("Ingrese de nuevo su PIN:\n ")
                if pin != pinConfirmacion:
                    print("Los PINS no coinciden!")
                else:
                    print("Registro exitoso")
                    usuariosRegistrados.append({
                        'idUsuario': idUsuario,
                        'nombre': nombre,
                        'pin': pin,
                        'saldo': 0
                    })
                    return
        intentos += 1
    print("Se excedió el máximo de intentos para ingresar un ID válido, volviendo al menú principal")

def depositarDinero():
    print("Depositar dinero")
    idUsuario = input("Ingrese su ID: ")
    pin = getpass.getpass("Ingrese su PIN: ")
    for usuario in usuariosRegistrados:
        if usuario['idUsuario'] == idUsuario and usuario['pin'] == pin:
            print(f"Bienvenido, {usuario['nombre']}!")
            divisa = int(input("Seleccione la divisa para el depósito:\n"
                            " 1) Colones\n"
                            " 2) Dólares\n"
                            " 3) Bitcoin\n"))
            montoDeposito = float(input("Indique el monto a depositar: "))
            if divisa == 1:
                tipoCambio = 1  # Conversión de colones a colones (1 a 1)
            elif divisa == 2:
                tipoCambio = 550  # Conversión de dólares a colones
            elif divisa == 3:
                tipoCambio = 26755  # Conversión de Bitcoin a colones
            else:
                print("Divisa no válida.")
                return
            montoColones = montoDeposito * tipoCambio
            usuario['saldo'] += montoColones
            print("Depósito exitoso")
            print("Saldo actual:", usuario['saldo'], "colones")
            return
    print("ID o PIN incorrecto")

def jugarBlackjack():
    saldo = saldoInicial

    while saldo >= apuestaMinima:
        print("¡Bienvenido al Blackjack de DreamWorld Casino!")
        print(f"Su saldo actual es: {saldo}")

        apuesta = int(input(f"Ingrese su apuesta (mínimo {apuestaMinima}): "))
        if apuesta < apuestaMinima or apuesta > saldo:
            print("Apuesta inválida.")
            continue

        naipe = [{'Naipe': naipe, 'Valor': valor} for naipe in naipes for valor in valores.values()]
        random.shuffle(naipe)

        manoJugador = [naipe.pop(), naipe.pop()]
        manoCrupier = [naipe.pop(), naipe.pop()]

        print("Tus cartas:")
        print(f"Carta: {manoJugador[0]['Naipe']} - Valor: {manoJugador[0]['Valor']}")
        print("Carta oculta")

        while True:
            accion = input("\n¿Desea pedir una nueva carta (P) o parar (S)? ").upper()

            if accion == 'P':
                manoJugador.append(naipe.pop())
                print(f"Carta: {manoJugador[-1]['Naipe']} - Valor: {manoJugador[-1]['Valor']}")

                if valorDeMano(manoJugador) > 21:
                    print("Te has pasado de 21. ¡Perdiste!")
                    saldo -= apuesta
                    break
            elif accion == 'S':
                break

        if valorDeMano(manoJugador) <= 21:
            print("Cartas del crupier:")
            print(f"Carta: {manoCrupier[0]['Naipe']} - Valor: {manoCrupier[0]['Valor']}")
            print("Carta oculta")

            while valorDeMano(manoCrupier) < 17:
                manoCrupier.append(naipe.pop())
                print(f"Carta: {manoCrupier[-1]['Naipe']} - Valor: {manoCrupier[-1]['Valor']}")

            print("Cartas del crupier:")
            for carta in manoCrupier:
                print(f"Carta: {carta['Naipe']} - Valor: {carta['Valor']}")

            if valorDeMano(manoCrupier) > 21 or valorDeMano(manoJugador) > valorDeMano(manoCrupier):
                print("¡Ganaste!")
                saldo += apuesta
            elif valorDeMano(manoJugador) < valorDeMano(manoCrupier):
                print("Perdiste.")
                saldo -= apuesta
            else:
                print("Empate.")

        print(f"Tu saldo actual es: {saldo}")
        jugarNuevamente = input("¿Quiere jugar nuevamente (S/N)? ").upper()
        if jugarNuevamente != 'S':
            print("¡Gracias por jugar con nosotros!")
            break

# Ciclo principal del programa
while True:
    mostrarMenu()
    opcion = input("Ingrese su opción: ")

    if opcion == "1":
        registrarUsuario()
    elif opcion == "2":
        depositarDinero()
    elif opcion == "3":
        print("Configuración Avanzada")
    elif opcion == "4":
        print("¡Hasta luego!")
        break
    else:
        print("Opción inválida. Por favor, ingrese un número del 1 al 4.")

