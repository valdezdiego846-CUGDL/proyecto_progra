import os
import datetime

# ==========================================
# VARIABLES GLOBALES Y LISTAS
# ==========================================
historial = []
MAX_HISTORIAL = 10
RUTA_ARCHIVO = "datos/historial.txt"

# ==========================================
# MANEJO DE ARCHIVOS Y PERSISTENCIA
# ==========================================
def cargar_historial():
    """Carga el historial desde el archivo de texto si existe."""
    if os.path.exists(RUTA_ARCHIVO):
        try:
            with open(RUTA_ARCHIVO, 'r', encoding='utf-8') as archivo:
                for linea in archivo:
                    historial.append(linea.strip())
        except Exception as e:
            print(f"Error al cargar el historial: {e}")

def guardar_historial():
    """Guarda las últimas operaciones en un archivo de texto."""
    if not os.path.exists("datos"):
        os.makedirs("datos")

    try:
        with open(RUTA_ARCHIVO, 'w', encoding='utf-8') as archivo:
            for operacion in historial:
                archivo.write(f"{operacion}\n")
    except Exception as e:
        print(f"Error al guardar el historial: {e}")

def agregar_al_historial(operacion, resultado):
    """Agrega una nueva entrada a la lista de historial."""
    fecha_actual = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    entrada = f"{fecha_actual} | {operacion} = {resultado}"

    historial.append(entrada)

    if len(historial) > MAX_HISTORIAL:
        historial.pop(0)

# ==========================================
# VALIDACIÓN DE ENTRADAS
# ==========================================
def validar_numero(mensaje, tipo=float):
    """Valida que la entrada del usuario sea un número válido."""
    while True:
        entrada = input(mensaje)
        try:
            numero = tipo(entrada)
            return numero
        except ValueError:
            print("Error: Entrada inválida. Ingresa un número válido.")

# ==========================================
# CALCULADORA BÁSICA
# ==========================================
def sumar(a, b): return a + b
def restar(a, b): return a - b
def multiplicar(a, b): return a * b

def dividir(a, b):
    if b == 0:
        return "Error: División por cero"
    return a / b

def modulo(a, b):
    if b == 0:
        return "Error: División por cero (Módulo)"
    return a % b

def potencia(a, b): return a ** b

def division_entera(a, b):
    if b == 0:
        return "Error: División por cero"
    return a // b

# ==========================================
# CONVERSOR DE DATOS
# ==========================================
def convertir_datos(valor, operacion):
    if operacion == "1": return f"{valor} B = {valor / 1024:.2f} KB"
    elif operacion == "2": return f"{valor} KB = {valor / 1024:.2f} MB"
    elif operacion == "3": return f"{valor} MB = {valor / 1024:.4f} GB"
    elif operacion == "4": return f"{valor} GB = {valor * 1024:.2f} MB"
    elif operacion == "5": return f"{valor} MB = {valor * 1024:.2f} KB"
    elif operacion == "6": return f"{valor} KB = {valor * 1024:.2f} B"
    else: return "Operación inválida"

# ==========================================
# SISTEMAS NUMÉRICOS
# ==========================================
def decimal_a_binario(n):
    if n == 0:
        return "0"

    binario = ""
    es_negativo = False

    if n < 0 and type(n) == int:
        es_negativo = True
        n = abs(n)

    while n > 0:
        residuo = n % 2
        binario = str(residuo) + binario
        n = n // 2

    return f"-{binario}" if es_negativo else binario

def decimal_a_hexadecimal(n):
    return hex(n)[2:].upper()

def binario_a_decimal(b):
    try:
        return str(int(b, 2))
    except ValueError:
        return "Error: Número binario inválido"

def hexadecimal_a_decimal(h):
    try:
        return str(int(h, 16))
    except ValueError:
        return "Error: Número hexadecimal inválido"

# ==========================================
# MENÚS
# ==========================================
def menu_basico():
    print("\n--- Calculadora Básica ---")
    operaciones = [
        "Suma (+)",
        "Resta (-)",
        "Multiplicación (*)",
        "División (/)",
        "División Entera (//)",
        "Módulo (%)",
        "Potencia (**)"
    ]

    for i, op in enumerate(operaciones, start=1):
        print(f"{i}. {op}")

    opcion = input("Selecciona una operación (1-7) o 'q' para cancelar: ")
    if opcion.lower() == 'q':
        return

    if opcion in [str(i) for i in range(1, 8)]:
        a = validar_numero("Ingresa el primer número: ")
        b = validar_numero("Ingresa el segundo número: ")

        if opcion == "1":
            resultado, simbolo = sumar(a, b), "+"
        elif opcion == "2":
            resultado, simbolo = restar(a, b), "-"
        elif opcion == "3":
            resultado, simbolo = multiplicar(a, b), "*"
        elif opcion == "4":
            resultado, simbolo = dividir(a, b), "/"
        elif opcion == "5":
            resultado, simbolo = division_entera(a, b), "//"
        elif opcion == "6":
            resultado, simbolo = modulo(a, b), "%"
        elif opcion == "7":
            resultado, simbolo = potencia(a, b), "**"

        cadena_op = f"{a} {simbolo} {b}"
        print(f"\nResultado: {cadena_op} = {resultado}")
        agregar_al_historial(cadena_op, str(resultado))
    else:
        print("Opción inválida.")

def menu_datos():
    print("\n--- Conversor de Unidades de Datos ---")
    opciones = [
        "1. Bytes a Kilobytes",
        "2. Kilobytes a Megabytes",
        "3. Megabytes a Gigabytes",
        "4. Gigabytes a Megabytes",
        "5. Megabytes a Kilobytes",
        "6. Kilobytes a Bytes"
    ]

    for op in opciones:
        print(op)

    opcion = input("Selecciona conversión (1-6) o 'q' para cancelar: ")

    if opcion in ["1", "2", "3", "4", "5", "6"]:
        valor = validar_numero("Ingresa el valor a convertir: ")
        resultado = convertir_datos(valor, opcion)
        print(f"\nResultado: {resultado}")
        agregar_al_historial("Conversión de Datos", resultado)
    elif opcion.lower() != 'q':
        print("Opción inválida.")

def menu_sistemas():
    print("\n--- Calculadora de Sistemas Numéricos ---")
    print("1. Decimal a Binario")
    print("2. Decimal a Hexadecimal")
    print("3. Binario a Decimal")
    print("4. Hexadecimal a Decimal")

    opcion = input("Elige una opción (1-4) o 'q' para cancelar: ")

    if opcion == "1":
        n = validar_numero("Ingresa número decimal (entero): ", int)
        res = decimal_a_binario(n)
        print(f"Binario: {res}")
        agregar_al_historial(f"Dec a Bin ({n})", res)

    elif opcion == "2":
        n = validar_numero("Ingresa número decimal (entero): ", int)
        res = decimal_a_hexadecimal(n)
        print(f"Hexadecimal: {res}")
        agregar_al_historial(f"Dec a Hex ({n})", res)

    elif opcion == "3":
        b = input("Ingresa número binario: ")
        res = binario_a_decimal(b)
        print(f"Decimal: {res}")
        agregar_al_historial(f"Bin a Dec ({b})", res)

    elif opcion == "4":
        h = input("Ingresa número hexadecimal: ")
        res = hexadecimal_a_decimal(h)
        print(f"Decimal: {res}")
        agregar_al_historial(f"Hex a Dec ({h})", res)

    elif opcion.lower() != 'q':
        print("Opción inválida.")

def menu_historial():
    print("\n--- Historial de Operaciones ---")
    print("1. Ver Historial")
    print("2. Limpiar Historial")
    opcion = input("Elige una opción (1-2): ")

    if opcion == "1":
        print("\n--- Últimas 10 Operaciones ---")
        if not historial:
            print("El historial está vacío.")
        else:
            for entrada in historial:
                print(entrada)

    elif opcion == "2":
        historial.clear()
        guardar_historial()
        print("\nHistorial limpiado correctamente.")
    else:
        print("Opción inválida.")

# ==========================================
# PROGRAMA PRINCIPAL
# ==========================================
def main():
    cargar_historial()
    print("¡Bienvenido a la Calculadora Python Multipropósito!")

    while True:
        print("\n" + "=" * 30)
        print("       MENÚ PRINCIPAL")
        print("=" * 30)
        print("1. Calculadora Básica")
        print("2. Conversor de Datos (B/KB/MB/GB)")
        print("3. Sistemas Numéricos (Bin/Hex/Dec)")
        print("4. Ver/Administrar Historial")
        print("5. Salir")
        print("=" * 30)

        seleccion = input("Selecciona una opción (1-5): ")

        if seleccion == "1":
            menu_basico()
        elif seleccion == "2":
            menu_datos()
        elif seleccion == "3":
            menu_sistemas()
        elif seleccion == "4":
            menu_historial()
        elif seleccion == "5":
            print("\nGuardando datos...")
            guardar_historial()
            print("¡Gracias por usar la calculadora! Hasta luego.")
            break
        else:
            print("\nError: Opción inválida. Inténtalo de nuevo.")

# ==========================================
# PUNTO DE ENTRADA
# ==========================================
if __name__ == "__main__":
    main()
