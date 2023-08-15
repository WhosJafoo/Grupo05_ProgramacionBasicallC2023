import random
import time
import getpass

usuariosRegistrados = []

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
        elif idUsuario in usuariosRegistrados:
            print("El ID ingresado ya está registrado")
        else:
            nombre = input("Ingrese su nombre: ")
            pin = getpass.getpass("Ingrese su PIN (mínimo de 6 dígitos): ")
            if len(pin) < 6:
                print("PIN inválido, mínimo seis dígitos.")
            else:
                pinConfirmacion = getpass.getpass("Ingrese de nuevo su PIN: ")
                if pin != pinConfirmacion:
                    print("Los PINS no coinciden!")
                else:
                    depositoMinimo = 1000

                    intentosDeposito = 3
                    while intentosDeposito > 0:
                        moneda = input("Ingrese la moneda de su depósito (dólares, colones o bitcoin): ")
                        montoDeposito = float(input("Ingrese el monto a depositar: "))

                        if moneda == "dólares":
                            if montoDeposito >= depositoMinimo:
                                usuariosRegistrados.append(idUsuario)
                                print("Registro exitoso. ¡Bienvenido al DreamWorld Casino!")
                                intentosDeposito = 0
                            else:
                                print(f"El depósito mínimo es de {depositoMinimo} dólares.")
                                intentosDeposito -= 1
                                if intentosDeposito > 0:
                                    print(f"Intentos restantes: {intentosDeposito}")
                        elif moneda == "colones":
                            tipoCambio = 0.001
                            montoDepositoDolares = montoDeposito * tipoCambio
                            if montoDepositoDolares >= depositoMinimo:
                                usuariosRegistrados.append(idUsuario)
                                print("Registro exitoso. ¡Bienvenido al DreamWorld Casino!")
                                intentosDeposito = 0
                            else:
                                print(f"El depósito mínimo es de {depositoMinimo} dólares.")
                                intentosDeposito -= 1
                                if intentosDeposito > 0:
                                    print(f"Intentos restantes: {intentosDeposito}")
                        elif moneda == "bitcoin":
                            tipoCambio = 35000
                            montoDepositoDolares = montoDeposito * tipoCambio
                            if montoDepositoDolares >= depositoMinimo:
                                usuariosRegistrados.append(idUsuario)
                                print("Registro exitoso. ¡Bienvenido al DreamWorld Casino!")
                                intentosDeposito = 0
                            else:
                                print(f"El depósito mínimo es de {depositoMinimo} dólares.")
                                intentosDeposito -= 1
                                if intentosDeposito > 0:
                                    print(f"Intentos restantes: {intentosDeposito}")
                        else:
                            print("Moneda no válida. Intente nuevamente.")
                    break
        intentos += 1
    print("Se excedió el máximo de intentos para ingresar un ID válido, volviendo al menú principal")

def mostrarInstrucciones():
    print("Instrucciones del juego:")
    print("1. Debes apostar al menos $100 para jugar.")
    print("2. Presiona la tecla J para jalar la palanca e iniciar el juego.")
    print("3. Cada ronda se mostrarán tres símbolos en pantalla.")
    print("4. Si aparecen tres @ en una ronda, recuperarás tu apuesta.")
    print("5. Si aparecen tres # en una ronda, ganarás el doble de tu apuesta.")
    print("6. Si aparecen tres 7 en una ronda, ganarás el acumulado.")
    print("7. El juego continuará hasta que decidas dejar de jugar.")

def jugarTragamonedas(saldo):
    if saldo < 100:
        print("Debes apostar al menos $100 para jugar.")
        return saldo

    print("¡Bienvenido(a) a la tragamonedas!")
    print("Tu saldo actual es de $", saldo)
    while True:
        inputJ = input("Presiona la tecla J para jalar la palanca: ")
        if inputJ == 'J' or inputJ == 'j':
            simbolos = ['@', '#', '+', '7']
            ronda = []
            for _ in range(3):
                simbolo = random.choice(simbolos)
                ronda.append(simbolo)
                print(simbolo, end=' ')
                time.sleep(1.5)
            print()

            if ronda.count('@') == 3:
                saldo += 100
                print("¡Felicidades! Recuperaste tu apuesta.")
            elif ronda.count('#') == 3:
                saldo *= 2
                print("¡Felicidades! Ganaste el doble de tu apuesta.")
            elif ronda.count('7') == 3:
                print("¡Felicidades! Ganaste el acumulado.")
                saldo += 1000
            else:
                saldo -= 100
                print("No tuviste suerte. Perdiste tu apuesta.")

            print("Tu saldo actual es de $", saldo)

            if saldo <= 0:
                print("Lo siento, te has quedado sin saldo.")
                break

            respuesta = input("¿Deseas seguir jugando? (S/N): ")
            if respuesta != 'S' and respuesta != 's':
                break
        else:
            print("Debes presionar la tecla J para jalar la palanca.")

def mostrarMenuCasino():
    print("DreamWorld Casino")
    print("A. Tragamonedas")
    print("B. BlackJack")
    print("C. Volver al menú principal")

def configuracionAvanzada():
    print("Configuración avanzada")
    global saldoTotal

    montoTotal = 0
    for moneda, monto in saldoTotal.items():
        montoTotal += monto

    nombreUsuario = input("Ingrese su nombre de usuario: ")
    print("Bienvenido,", nombreUsuario)

    opcionUsu = int(input("Indique la transacción que desea utilizar:\n"
                          "1) Retirar Dinero\n"
                          "

