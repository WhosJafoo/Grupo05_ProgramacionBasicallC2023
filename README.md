# Grupo05_ProgramacionBasicallC2023
Proyecto programación segundo cuatrimestre

import random
import time
import os
import getpass


# Código del juego de Blackjack
naipes = ['Corazones', 'Diamantes', 'Picas', 'Tréboles']
valores = {'Dos': 2, 'Tres': 3, 'Cuatro': 4, 'Cinco': 5, 'Seis': 6, 'Siete': 7,
           'Ocho': 8, 'Nueve': 9, 'Diez': 10, 'Jota': 10, 'Reina': 10, 'Rey': 10, 'As': 11}
saldoInicial = 100
apuestaMinima = 10


class DreamWorldCasino:

    def __init__(self):
        self.usuariosRegistrados = [
            {"id": "Jafet", "pin": "123456"},
            {"id": "Usuario1", "pin": "789012"}
        ]
        self.montoDeposito = 1000
        self.depositoObligatorio = 100

        self.idUsuario = "Usuario1"
        self.pinUsuario = "789012"

        self.iniciar()

    def iniciar(self):
        while True:
            print("Bienvenido al Menú:")
            print("1. Registro de Usuario Nuevo")
            print("2. DreamWorld Casino")
            print("3. Configuración Avanzada")
            print("4. Depósito Obligatorio")
            print("5. Salir")

            opcion = input("Selecciona una opción (1-5): ")

            if opcion == "1":
                self.registroUsuarioNuevo()
            elif opcion == "2":
                self.submenu_Casino()
            elif opcion == "3":
                self.configuracionAvanzada()
            elif opcion == "4":
                self.depositoObligatorio()
            elif opcion == "5":
                self.salir()
                break
            else:
                print("Opción no válida. Por favor, selecciona una opción del 1 al 5.")

    def guardarInfoUsuario(self, idUsuario):
        carpetaUsuario = f"usuario_{idUsuario}"
        os.makedirs(carpetaUsuario, exist_ok=True)
        with open(os.path.join(carpetaUsuario, "informacion.txt"), "w") as archivo:
            archivo.write(f"Información del usuario {idUsuario}")

    def eliminar_Usuario(self):
        ingreso_id = input("Ingresa tu nombre de usuario: ")
        ingreso_pin = getpass.getpass("Ingresa tu PIN: ")

        if ingreso_id == self.idUsuario and ingreso_pin == self.pinUsuario:
            if self.montoDeposito > 0:
                print("Debes retirar tu saldo antes de eliminar la cuenta.")
            else:
                confirmacion = input(
                    "¿Estás seguro de eliminar tu cuenta? (S/N): ")
                if confirmacion.lower() == "s":
                    for usuario in self.usuariosRegistrados:
                        if usuario["id"] == self.idUsuario:
                            self.usuariosRegistrados.remove(usuario)
                            print("Cuenta eliminada exitosamente.")
                            return
        else:
            print("Credenciales incorrectas. No se pudo eliminar la cuenta.")

    def obtenerMontoMinino(self):
        return self.depositoObligatorio

    def submenu_Casino(self):
        while True:
            print("1. Retirar Dinero")
            print("2. Depositar Dinero")
            print("3. Ver Saldo Actual")
            print("4. Juegos en linea")
            print("5. Eliminar Usuario")
            print("6. Salir")

            opcionUsu = int(input("Seleccione una opción: "))

            if opcionUsu == 1:
                print(
                    "El monto actual de dinero que tiene disponible es de ", self.montoDeposito)

                resMonto = float(input("Digite el monto que desea retirar: "))
                if resMonto > self.montoDeposito:
                    print(
                        "El monto que desea retirar es mayor al disponible. No se puede realizar la transacción.")
                else:
                    self.montoDeposito -= resMonto
                    print("El monto que retiró es de ", resMonto)
                    print("El monto que quedó disponible es de ¢",
                          self.montoDeposito, "colones")

            elif opcionUsu == 2:
                diviDis = int(input("Bienvenido al menú para depositar dinero. Seleccione la divisa:\n"
                                    "1) Colones\n"
                                    "2) Dólares\n"
                                    "3) Bitcoin\n"))

                if diviDis == 1:
                    print("La divisa que gusta utilizar es colones")
                    depoColo = float(
                        input("Digite la cantidad de colones que desea depositar: "))
                    print("El monto que depositó es de ¢", depoColo, "colones")

                elif diviDis == 2:
                    print("La divisa que gusta utilizar es el dólar")
                    depoDola = float(
                        input("Indique el monto que desea depositar: "))
                    conver = depoDola * 550
                    print("El monto que depositó en dólares es de $", depoDola)
                    print("Este monto equivale a ¢", conver, "colones")

                elif diviDis == 3:
                    print("La divisa seleccionada es el Bitcoin")
                    depoBit = float(
                        input("Digite el monto que desea depositar: "))
                    converBit = depoBit * 26755
                    converColo = converBit * 550
                    print("El monto que depositó en Bitcoin es de", depoBit)
                    print("Este monto equivale a $", converBit, "dólares")
                    print("Este monto equivale a ¢", converColo, "colones")

                else:
                    print(
                        "No existe esa opción. Por favor, seleccione una opción correcta.")

            elif opcionUsu == 3:
                print("Su saldo actual es de", self.montoDeposito)

            elif opcionUsu == 4:
                while True:
                    print(
                        "Los juegos disponibles son:\n1. Black Jack\n2. Tragamonedas\n3. SALIR")
                    opcionJuego = input("Selecciona un juego: ")

                    if opcionJuego == "1":
                        self.jugarBlackjack()
                    elif opcionJuego == "2":
                        self.menu_Traga()
                    elif opcionJuego == "3":
                        break
                    else:
                        print(
                            "Opción inválida. Por favor, selecciona una opción válida.")

            elif opcionUsu == 5:
                self.eliminar_Usuario()

            elif opcionUsu == 6:
                print("Gracias por su tiempo.")
                break

    def jugarBlackjack(self):
        saldo = saldoInicial
        while saldo >= apuestaMinima:
            print("¡Bienvenido al Blackjack de DreamWorld Casino!")
            print(f"Su saldo actual es: {saldo}")

            apuesta = int(
                input(f"Ingrese su apuesta (mínimo {apuestaMinima}): "))
            if apuesta < apuestaMinima or apuesta > saldo:
                print("Apuesta inválida.")
                continue

            naipe = crearMano()
            mezclarNaipe(naipe)

            manoJugador = [naipe.pop(), naipe.pop()]
            manoCrupier = [naipe.pop(), naipe.pop()]

            mostrarMano(manoJugador)
    # ciclo que se termina hasta que se llegue a 21
            while True:
                accion = input(
                    "\n¿Desea pedir una nueva carta (P) o parar (S)? ").upper()
    # con la letra P se le da la opcion al jugador de poder continuar y con la S se retira del siguiente juego
                if accion == 'P':
                    manoJugador.append(naipe.pop())
                    mostrarMano(manoJugador)
                    if valorDeMano(manoJugador) > 21:
                        print("Te has pasado de 21. ¡Perdiste!")
                        saldo -= apuesta
                        break
                elif accion == 'S':
                    break
    # el break es para salir de la partida
            if valorDeMano(manoJugador) <= 21:
                mostrarMano(manoCrupier, ocultarPrimeraCarta=True,
                            jugador=False)
    # el .pop es para eliminar y poner de vuelta el ultimo elemento, por decir quita la primera carta de la lista, por decir que tomo una carta
    # del mazo y la puso en la mano del jugador o del crupier
                while valorDeMano(manoCrupier) < 17:
                    manoCrupier.append(naipe.pop())

                mostrarMano(manoCrupier, jugador=False)

                if valorDeMano(manoCrupier) > 21 or valorDeMano(manoJugador) > valorDeMano(manoCrupier):
                    print("¡Ganaste!")
                    saldo += apuesta
                elif valorDeMano(manoJugador) < valorDeMano(manoCrupier):
                    print("Perdiste.")
                    saldo -= apuesta
                else:
                    print("Empate.")
    # el upper es para usar mayusculas en la opcion que seleccione
            print(f"Tu saldo actual es: {saldo}")
            jugarNuevamente = input("Quiere jugar nuevamente (S/N)? ").upper()
            if jugarNuevamente != 'S':
                print("¡Gracias por jugar con nosotros!")
                break

    def jugar_Tragamonedas(self):
        if self.montoDeposito < 100:
            print("Debes apostar al menos $100 para jugar.")
            return self.montoDeposito
    # el input lleva la j que se usa para JUGAR sea J o j
        print("¡Bienvenido(a) a la tragamonedas!")
        print("Tu saldo actual es de $", self.montoDeposito)

        while True:
            input_j = input("Presiona la tecla J para jalar la palanca: ")
            if input_j == 'J' or input_j == 'j':
                simbolos = ['@', '#', '+', '7']
                ronda = []

                for _ in range(3):
                    simbolo = random.choice(simbolos)
                    ronda.append(simbolo)
                    print(simbolo, end=' ')
                    time.sleep(1.5)
                print()

                if ronda.count('@') == 3:
                    self.montoDeposito += 100
                    print("¡Felicidades! Recuperaste tu apuesta.")
                elif ronda.count('#') == 3:
                    self.montoDeposito *= 2
                    print("¡Felicidades! Ganaste el doble de tu apuesta.")
                elif ronda.count('7') == 3:
                    print("¡Felicidades! Ganaste el acumulado.")
                    self.montoDepositoo += 1000
                else:
                    self.montoDeposito -= 100
                    print("No tuviste suerte. Perdiste tu apuesta.")

                print("Tu saldo actual es de $", self.montoDeposito)

                if self.montoDeposito <= 0:
                    print("Lo siento, te has quedado sin saldo.")
                    break

                respuesta = input("¿Deseas seguir jugando? (S/N): ")
                if respuesta != 'S' and respuesta != 's':
                    break
            else:
                print("Debes presionar la tecla J para jalar la palanca.")

        return self.montoDeposito
        pass

    def menu_Traga(self):
        saldo = 1000  # Saldo inicial del jugador

        while True:
            print("\n==== Menú Principal ====")
            print("1. Mostrar instrucciones")
            print("2. Jugar a la tragamonedas")
            print("3. Salir")

            opcion = input("Selecciona una opción: ")

            if opcion == '1':
                mostrar_instrucciones()
            elif opcion == '2':
                self.jugar_Tragamonedas()
            elif opcion == '3':
                print("Gracias por jugar. ¡Hasta luego!")
                break
            else:
                print("Opción inválida. Por favor, selecciona una opción válida.")
        pass

    def depositoObligatorio(self):
        print("Opción 4: Depósito Obligatorio")
        intentos = 3
        while intentos > 0:
            print("Realiza un depósito mínimo para continuar.")
            # Función para obtener el monto mínimo desde la configuración
            montoMinimo = self.obtenerMontoMinimo()

            monedaDeposito = input(
                "Selecciona la moneda de depósito (dólares, colones, bitcoin): ").lower()
            if monedaDeposito == "dólares":
                monedaDeposito = "USD"
            elif monedaDeposito == "colones":
                monedaDeposito = "CRC"
            elif monedaDeposito == "bitcoin":
                monedaDeposito = "BTC"
            else:
                print("Moneda no válida. Por favor, elige una moneda válida.")
                continue

            montoDeposito = float(
                input(f"Ingrese el monto de depósito en {monedaDeposito}: "))

            if monedaDeposito != "USD":
                # Aquí debería ir la lógica para convertir el monto a dólares usando el tipo de cambio
                pass

            if montoDeposito < montoMinimo:
                print(
                    f"El monto mínimo de depósito es {montoMinimo} {monedaDeposito}.")

                intentos -= 1
                if intentos > 0:
                    print(f"Te quedan {intentos} intentos.")
                else:
                    print(
                        "Se excedió el máximo de intentos para depositar el mínimo de dinero requerido, volviendo al menú principal.")
                continue

            print(f"¡Depósito de {montoDeposito} {monedaDeposito} exitoso!")
            break

    def salir(self):
        print("Gracias por visitar DreamWorld Casino. ¡Hasta luego!")

    def registroUsuarioNuevo(self):
        print("Opción 1: Registro de Usuario Nuevo")
        intentos = 3
        while intentos > 0:
            idUsuario = input(
                "Ingrese su nombre de usuario o ID (mínimo cinco caracteres alfanuméricos): ")
            if len(idUsuario) < 5:
                print("El ID debe tener al menos cinco caracteres.")
                intentos -= 1
                if intentos > 0:
                    print(f"Te quedan {intentos} intentos.")
                else:
                    print(
                        "Se excedió el máximo de intentos para ingresar un ID válido, volviendo al menú principal.")
                continue

            if idUsuario in self.usuariosRegistrados:
                print("El ID ingresado ya está registrado. Por favor, elige otro.")
                intentos -= 1
                if intentos > 0:
                    print(f"Te quedan {intentos} intentos.")
                else:
                    print(
                        "Se excedió el máximo de intentos para ingresar un ID válido, volviendo al menú principal.")
                continue

            pinUsuario = getpass.getpass(
                "Ingrese su PIN (autenticador único): ")
            confirmarPin = getpass.getpass("Confirme su PIN: ")

            if pinUsuario != confirmarPin:
                print("Los PIN no coinciden. Por favor, ingrese nuevamente.")
                intentos -= 1
                if intentos > 0:
                    print(f"Te quedan {intentos} intentos.")
                else:
                    print(
                        "Se excedió el máximo de intentos para ingresar un PIN válido, volviendo al menú principal.")
                continue

            self.usuariosRegistrados.append(idUsuario)
            print(
                f"¡Registro exitoso! Bienvenido, {idUsuario}, al DreamWorld Casino.")
            self.guardarInfoUsuario(idUsuario)
            break
        usuarios = int(input("Digite cuantos usuarios va a a acceder: \n"))


def agregarNombre():
    file = open("registroUsuario.txt", "w")
    file.write("Su nombre de usuario es: \n")
    informacionJugador = input("Digite el nombre que desea para utilizar: \n")
    file.write(informacionJugador)
    file.close()


def mostrarInformacion():
    try:
        file = open("registroUsuario.txt", "r")
        informacionJugador = file.read()
        file.close()
        print(informacionJugador)
    except IOError:
        print("El archivo NO existe")


def crearMano():
    return [{'Naipe': naipe, 'Valor': valor} for naipe in naipes for valor in valores.values()]


def mezclarNaipe(naipe):
    random.shuffle(naipe)

# aqui se calcula el valor de la mano(las cartas y su valor)


def valorDeMano(mano):
    valor = sum([carta['Valor'] for carta in mano])
    numAses = sum([1 for carta in mano if carta['Naipe'] == 'As'])

    while valor > 21 and numAses:
        valor -= 10
        numAses -= 1

    return valor

# mostrar mano de jugador o del crupier


def mostrarMano(mano, ocultarPrimeraCarta=False, jugador=True, revelarSegundaCarta=False):
    if jugador:
        print("Tus cartas:")
    else:
        print("Cartas del crupier:")

    for i, carta in enumerate(mano):
        if (i == 0 and ocultarPrimeraCarta) and jugador:
            print("Carta oculta")
        elif (i == 1 and ocultarPrimeraCarta) and not jugador and not revelarSegundaCarta:
            print("Carta oculta")
        else:
            print(f"Carta: {carta['Naipe']} - Valor: {carta['Valor']}")
# Función para el juego de Blackjack


# Código del juego de Tragamonedas
def mostrar_instrucciones():
    print("Instrucciones del juego:")
    print("1. Debes apostar al menos $100 para jugar.")
    print("2. Presiona la tecla J para jalar la palanca e iniciar el juego.")
    print("3. Cada ronda se mostrarán tres símbolos en pantalla.")
    print("4. Si aparecen tres @ en una ronda, recuperarás tu apuesta.")
    print("5. Si aparecen tres # en una ronda, ganarás el doble de tu apuesta.")
    print("6. Si aparecen tres 7 en una ronda, ganarás el acumulado.")
    print("7. El juego continuará hasta que decidas dejar de jugar.")
    pass


# Código del menú principal y submenú del casino


def menu_principal():
    while True:
        print("\n==== Menú Principal ====")
        print("1. Registrar usuarios")
        print("2. DreamWorld Casino")
        print("3. Configuración avanzada")
        print("4. Salir")

        opcionUsu = int(input("Selecciona una opción: "))

        if opcionUsu == 1:
            print("Opción: Registrar usuarios")

        elif opcionUsu == 2:
            print("Opción: DreamWorld Casino")
            submenu_casino()
        elif opcionUsu == 3:
            print("Opción: Configuración avanzada")
            # Agregar aquí el código para la configuración avanzada si es necesario
        elif opcionUsu == 4:
            print("Saliendo...")
            break
        else:
            print("Opción inválida. Por favor, selecciona una opción válida.")


def submenu_casino():
    while True:
        print("\n==== DreamWorld Casino ====")
        print("1. Jugar al Blackjack")
        print("2. Jugar a la Tragamonedas")
        print("3. Volver al menú principal")

        opcionJuego = int(input("Selecciona un juego: "))

        if opcionJuego == 1:
            print("¡Bienvenido al Blackjack de DreamWorld Casino!")
            jugar_Blackjack()
        elif opcionJuego == 2:
            print("¡Bienvenido(a) a la tragamonedas!")
            menu_Traga(self)
        elif opcionJuego == 3:
            break
        else:
            print("Opción inválida. Por favor, selecciona una opción válida.")


def main():
    menu_principal()


if __name__ == "__main__":
    casino = DreamWorldCasino()
    casino.iniciar()
