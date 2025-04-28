# ENCAPSULAMIENTO

## Ejercicio 1

class Estudiante:

    def __init__(self, nombre, codigo):
        self.__nombre = None
        self.__codigo = None
        self.__notas = []
        self.nombre = nombre  # Usamos el setter
        self.codigo = codigo  # Usamos el setter
        
    @property
    def nombre(self):
        return self.__nombre
        
    @nombre.setter
    def nombre(self, nuevo_nombre):
        if isinstance(nuevo_nombre, str) and nuevo_nombre.strip():
            self.__nombre = nuevo_nombre.strip()
        else:
            raise ValueError("El nombre no puede estar vacío.")
            
    @property
    def codigo(self):
        return self.__codigo
    @codigo.setter
    
    def codigo(self, nuevo_codigo):
        if isinstance(nuevo_codigo, str) and nuevo_codigo.isalnum():
            self.__codigo = nuevo_codigo
        else:
            raise ValueError("El código debe ser alfanumérico.")
            
    def agregar_nota(self, nota):
        if isinstance(nota, (int, float)) and 0.0 <= nota <= 5.0:
            self.__notas.append(float(nota))
        else:
            raise ValueError("La nota debe estar entre 0.0 y 5.0.")   
            
    def calcular_promedio(self):
        if self.__notas:
            return sum(self.__notas) / len(self.__notas)
        return 0.0  
        
    def es_aprobado(self):
        return self.calcular_promedio() >= 3.0

### TESTING
e1 = Estudiante("Cristian Conde", "A1")
e1.agregar_nota(4.0)
e1.agregar_nota(5.0)
e1.agregar_nota(4.5)

print("Nombre:", e1.nombre)
print("Código:", e1.codigo)
print("Promedio:", e1.calcular_promedio())
print("¿Aprobado?:", e1.es_aprobado())


## Ejercicio 2

class CarteraCripto:

    def __init__(self, usuario):
        self.__usuario = usuario
        self.__saldo_btc = 0.0  # Saldo inicial en BTC
        
    def consultar_saldo(self):
        print(f"Usuario: {self.__usuario}")
        print(f"Saldo BTC: {self.__saldo_btc:.8f} BTC")
        
    def comprar_btc(self, monto_usd, precio_actual_btc):
        if monto_usd > 0 and precio_actual_btc > 0:
            btc_comprado = monto_usd / precio_actual_btc
            self.__saldo_btc += btc_comprado
            print(f"Compra exitosa: {btc_comprado:.8f} BTC añadidos.")
        else:
            print("Monto en USD y precio de BTC deben ser positivos.")
            
    def vender_btc(self, monto_btc, precio_actual_btc):
        if monto_btc > 0 and precio_actual_btc > 0:
            if monto_btc <= self.__saldo_btc:
                usd_recibido = monto_btc * precio_actual_btc
                self.__saldo_btc -= monto_btc
                print(f"Venta exitosa: recibiste ${usd_recibido:.2f} USD.")
            else:
                print("Error: No tienes suficiente BTC para vender.")
        else:
            print("Monto de BTC y precio actual deben ser positivos.")
            
### TESTING

#Crear cartera
cartera = CarteraCripto("Cristian")

#Consultar saldo inicial
cartera.consultar_saldo()

#Comprar BTC
cartera.comprar_btc(monto_usd=1000, precio_actual_btc=50000)  # Compra con 1000 USD

#Consultar saldo después de la compra
cartera.consultar_saldo()

#Vender BTC
cartera.vender_btc(monto_btc=0.01, precio_actual_btc=60000)  # Vende 0.01 BTC

#Consultar saldo final
cartera.consultar_saldo()


## Ejercicio 3

class Empleado:

    def __init__(self, nombre, rol, clave):
        self.__nombre = nombre
        self.__rol = rol
        self._clave_acceso = self._cifrar(clave)
        
    def _cifrar(self, texto):
        return texto[::-1]
        
    def _decifrar(self, clave_encriptada):
        return clave_encriptada[::-1]  # Invertir la clave
        
    @property
    def nombre(self):
        return self.__nombre
        
    @property
    def rol(self):
        return self.__rol
        
    def verificar_clave(self, clave_ingresada):
        return self._cifrar(clave_ingresada) == self._clave_acceso
        
    def cambiar_clave(self, clave_antigua, nueva_clave):
        if self.verificar_clave(clave_antigua):
            self._clave_acceso = self._cifrar(nueva_clave)
            return True
        return False
        
### TESTING
emp = Empleado("Fabian", "supervisor", "Hellokitty")
print(emp.verificar_clave("Hellokitty"))  
emp.cambiar_clave("Hellokitty", "Kdrama")
print(emp.verificar_clave("Kdrama"))     


## Ejercicio 4

class Persona:

    def __init__(self, nombre, edad, documento):
        self.__nombre = None
        self.__edad = None
        self.__documento = None

        self.nombre = nombre     # Usamos setter
        self.edad = edad         # Usamos setter
        self.documento = documento  # Usamos setter

    # Getter y setter para nombre
    @property
    def nombre(self):
        return self.__nombre

    @nombre.setter
    def nombre(self, nuevo_nombre):
        if isinstance(nuevo_nombre, str) and nuevo_nombre.strip():
            self.__nombre = nuevo_nombre.strip()
        else:
            raise ValueError("El nombre no puede estar vacío.")

    # Getter y setter para edad
    @property
    def edad(self):
        return self.__edad

    @edad.setter
    def edad(self, nueva_edad):
        if isinstance(nueva_edad, int) and nueva_edad >= 0:
            self.__edad = nueva_edad
        else:
            raise ValueError("La edad debe ser un número entero mayor o igual a 0.")

    # Getter y setter para documento
    @property
    def documento(self):
        return self.__documento

    @documento.setter
    def documento(self, nuevo_documento):
        if isinstance(nuevo_documento, str) and nuevo_documento.strip():
            self.__documento = nuevo_documento.strip()
        else:
            raise ValueError("El documento no puede estar vacío.")

#Clase hija Paciente
class Paciente(Persona):

    def __init__(self, nombre, edad, documento, diagnostico):
        super().__init__(nombre, edad, documento)
        self.__diagnostico = diagnostico
        self.__historial = []

    def agregar_historial(self, entrada):
        if isinstance(entrada, str) and entrada.strip():
            self.__historial.append(entrada.strip())
        else:
            raise ValueError("La entrada del historial debe ser un texto no vacío.")

    def ver_historial(self):
        return self.__historial

    def ver_diagnostico(self):
        return self.__diagnostico

    # Método para modificar el diagnóstico (acceso controlado por Doctor)
    def _modificar_diagnostico(self, nuevo_diagnostico):
        self.__diagnostico = nuevo_diagnostico

# Clase hija Doctor
class Doctor(Persona):

    def __init__(self, nombre, edad, documento, especialidad):
        super().__init__(nombre, edad, documento)
        self.__especialidad = especialidad

    def ver_especialidad(self):
        return self.__especialidad

    def modificar_diagnostico(self, paciente, nuevo_diagnostico):
        if isinstance(paciente, Paciente):
            paciente._modificar_diagnostico(nuevo_diagnostico)
            print(f"Diagnóstico actualizado para {paciente.nombre}.")
        else:
            print("Error: Solo se puede modificar el diagnóstico de un Paciente.")

###TESTING

#Crear un paciente
paciente1 = Paciente("Juan Pérez", 30, "123456789", "Gripe")

#Agregar entradas al historial
paciente1.agregar_historial("Visita inicial: fiebre alta")
paciente1.agregar_historial("Segunda visita: mejoría")

#Crear un doctor
doctor1 = Doctor("Dra. García", 45, "987654321", "Medicina General")

#Ver información del paciente
print(f"Nombre del paciente: {paciente1.nombre}")
print(f"Diagnóstico actual: {paciente1.ver_diagnostico()}")
print("Historial médico:", paciente1.ver_historial())

#Doctor modifica el diagnóstico
doctor1.modificar_diagnostico(paciente1, "Recuperado - Alta médica")

#Verificar cambios
print(f"Nuevo diagnóstico: {paciente1.ver_diagnostico()}")



# POLIMORFISMO


## Ejercicio 1 

class Empleado:

    def __init__(self, nombre, sueldo_base):
        self.nombre = nombre
        self.sueldo_base = sueldo_base
        
    def calcular_salario(self):
        pass  # Método que será sobrescrito por las subclases

class EmpleadoFijo(Empleado):

    def calcular_salario(self):
        bono = 500
        return self.sueldo_base + bono

class EmpleadoPorHoras(Empleado):

    def __init__(self, nombre, sueldo_base, horas_trabajadas):
        super().__init__(nombre, sueldo_base)
        self.horas_trabajadas = horas_trabajadas
        
    def calcular_salario(self):
        return self.sueldo_base * self.horas_trabajadas

class EmpleadoTemporal(Empleado):

    def calcular_salario(self):
        return self.sueldo_base * 0.8

### TESTING
print("=== Cálculo de salarios con polimorfismo ===")

empleados = [
    EmpleadoFijo("Ana", 1500),
    EmpleadoPorHoras("Luis", 15, 120),
    EmpleadoTemporal("María", 2000)
]

for emp in empleados:
    print(f"{emp.nombre} gana: ${emp.calcular_salario():.2f}")

## Ejercicio 2


class Transporte:

    def tipo_transporte(self):
        print("Tipo de transporte genérico")

class Coche(Transporte):

    def tipo_transporte(self):
        print("Transporte terrestre")

class Avion(Transporte):

    def tipo_transporte(self):
        print("Transporte aéreo")

class Barco(Transporte):

    def tipo_transporte(self):
        print("Transporte marítimo")

### TESTING 
transportes = [Coche(), Avion(), Barco()]

for t in transportes:
    t.tipo_transporte()

