clientes =[]
pedidos =[]
def registrar_cliente():
    nombre = input("Ingrese el nombre del cliente: ")
    direccion = input("Ingrese la dirección del cliente: ")
    pais = input("Ingrese el país del cliente: ")
    email = input("Ingrese el email del cliente: ")
    telefono = input("Ingrese el número de teléfono del cliente: ")

    nuevo_cliente = {"nombre": nombre, "direccion": direccion, "pais": pais, "email": email, "telefono": telefono}
    clientes.append(nuevo_cliente)

    print(f"Cliente {nombre} registrado con éxito.")

def visualizar_clientes(clientes):
    for cliente in clientes:
        print(f"Nombre: {cliente['nombre']}, País: {cliente['pais']}, Email: {cliente['email']}")

def buscar_cliente(clientes, nombre):
    for cliente in clientes:
        if cliente['nombre'] == nombre:
            print(f"Cliente encontrado - Nombre: {cliente['nombre']}, País: {cliente['pais']}, Email: {cliente['email']}")
            return cliente
    print(f"No se encontró ningún cliente con el nombre {nombre}.")
    return None

def realizar_compra(clientes, pedidos):
    nombre_cliente = input("Ingrese el nombre del cliente que realiza la compra: ")
    cliente = buscar_cliente(clientes, nombre_cliente)

    if cliente:
        productos_compra = input("Ingrese los productos de la compra (separados por comas): ").split(',')
        nuevo_pedido = {"cliente": cliente, "productos": productos_compra,}
        pedidos.append(nuevo_pedido)
        
def enviar_sms(cliente, mensaje):
    print(f"SMS enviado a {cliente['telefono']}: {mensaje}")

def enviar_correo(cliente, asunto, mensaje):
    print(f"Correo enviado a {cliente['email']} - Asunto: {asunto}, Mensaje: {mensaje}")

def guardar_clientes(clientes):
    with open("clientes.txt", "w") as file:
        for cliente in clientes:
            file.write(f"{cliente['nombre']},{cliente['direccion']},{cliente['pais']},{cliente['email']},{cliente['telefono']}\n")

def cargar_clientes():
    clientes = []
    try:
        with open("clientes.txt", "r") as arch:
            for line in arch:
                data = line.strip().split(',')
                cliente = {"nombre": data[0], "direccion": data[1], "pais": data[2], "email": data[3], "telefono": data[4]}
                clientes.append(cliente)
    except FileNotFoundError:
        pass
    return clientes

def guardar_pedidos(pedidos):
    with open("pedidos.txt", "w") as arch:
        for pedido in pedidos:
            arch.write(f"{pedido['cliente']['nombre']},{','.join(pedido['productos'])},{pedido['codigo_seguimiento']}\n")

def cargar_pedidos(clientes):
    pedidos = []
    try:
        with open("pedidos.txt", "r") as arch:
            for line in arch:
                data = line.strip().split(',')
                nombre_cliente = data[0]
                productos = data[1].split(',')
                codigo_seguimiento = data[2]
                cliente = buscar_cliente(clientes, nombre_cliente)
                if cliente:
                    nuevo_pedido = {"cliente": cliente, "productos": productos, "codigo_seguimiento": codigo_seguimiento}
                    pedidos.append(nuevo_pedido)
    except FileNotFoundError:
        pass
    return pedidos

def main():
    clientes = cargar_clientes(pedidos)
    pedidos = cargar_pedidos(clientes)

while True:
    print("\n*** Menú ***")
    print("1. Registrar Cliente")
    print("2. Visualizar Clientes")
    print("3. Realizar Compra")
    print("4. Salir")

    opcion = input("Seleccione una opción: ")

    if opcion == "1":
        registrar_cliente()
    elif opcion == "2":
        visualizar_clientes(clientes)
    elif opcion == "3":
        realizar_compra(clientes, pedidos)
    elif opcion == "4":
        guardar_clientes(clientes)
        guardar_pedidos(pedidos)
        print("¡Hasta luego!")
        break
    else:
        print("Opción no válida. Inténtelo de nuevo.")
