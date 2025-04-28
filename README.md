#ENCAPSULAMIENTO

# Ejercicio l

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

### TESTING:

emp = Empleado("Fabian", "supervisor", "Hellokitty")
print(emp.verificar_clave("Hellokitty"))  # Debería imprimir True
emp.cambiar_clave("Hellokitty", "Kdrama")
print(emp.verificar_clave("Kdrama"))      # Debería imprimir True



# Ejercicio 2

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



# POLIMORFISMO


# Ejercicio 1 

class Empleado:
    def __init__(self, nombre, sueldo_base):
        self.nombre = nombre
        self.sueldo_base = sueldo_base
    def calcular_salario(self):
        pass  # Método que será sobreescrito por las subclases

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

print("=== Cálculo de salarios con polimorfismo ===")
empleados = [
    EmpleadoFijo("Ana", 1500),
    EmpleadoPorHoras("Luis", 15, 120),
    EmpleadoTemporal("María", 2000)
]

for emp in empleados:
    print(f"{emp.nombre} gana: ${emp.calcular_salario():.2f}")
