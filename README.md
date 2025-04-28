#ENCAPSULAMIENTO

#Ejercicio l

class Empleado:
    def _init_(self, nombre, rol, clave ):
        self.__nombre = nombre
        self.__rol = rol
        self._clave_acceso = self._cifrar(clave)
    def __cifrar(self, texto):
        return texto[::-1]
    
#Metodos de incriptacion y desencriptacion de la clave 
    def __decifrar (self, clave_incriptada):
        return clave_incriptada[::-1] # Invertir la clave, es decir, si entra hola sale "aloh"
    
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
    #TESTING:

emp = Empleado("Fabian", "sapovisor", "Hellokitty")
print(emp.verificar_clave("Hellokitty"))
emp.cambiar_clave("Hellokitty", "Kdrama")
print(emp.verificar_clave("Kdrama"))


#Ejercicio 2

class Estudiante:
    def _init_(self, nombre, codigo):
        self.__nombre = None
        self.__codigo = None
        self.__notas = []
        self.nombre = nombre  # Usamos el setter
        self.codigo = codigo  # Usamos el setter

    # Getter y Setter para nombre
    @property
    def nombre(self):
        return self.__nombre

    @nombre.setter
    def nombre(self, nuevo_nombre):
        if isinstance(nuevo_nombre, str) and nuevo_nombre.strip():
            self.__nombre = nuevo_nombre.strip()
        else:
            raise ValueError("El nombre no puede estar vacío.")

    # Getter y Setter para codigo
    @property
    def codigo(self):
        return self.__codigo

    @codigo.setter
    def codigo(self, nuevo_codigo):
        if isinstance(nuevo_codigo, str) and nuevo_codigo.isalnum():
            self.__codigo = nuevo_codigo
        else:
            raise ValueError("El codigo debe ser alfanumerico.")

    # Método para agregar una nota
    def agregar_nota(self, nota):
        if isinstance(nota, (int, float)) and 0.0 <= nota <= 5.0:
            self.__notas.append(float(nota))
        else:
            raise ValueError("La nota debe estar entre 0.0 y 5.0.")

    # Metodo para calcular el promedio
    def calcular_promedio(self):
        if self.__notas:
            return sum(self._notas) / len(self._notas)
        return 0.0

    # Metodo para saber si el estudiante esta aprobado
    def es_aprobado(self):
        return self.calcular_promedio() >= 3.0

# TESTING
e1 = Estudiante("Cristian Conde", "A1")
e1.agregar_nota(4.0)
e1.agregar_nota(5.0)
e1.agregar_nota(4.5)

print("Nombre:", e1.nombre)
print("Codigo:", e1.codigo)
print("Promedio:", e1.calcular_promedio())
print("¿Aprobado?:", e1.es_aprobado())

#POLIMORFISMO
#Ejercicio 1 
[8:10 p. m., 21/4/2025] Conde: # Clase base
class Empleado:
    def _init_(self, nombre, sueldo_base):
        self.__nombre = nombre
        self.__sueldo_base = sueldo_base

    @property
    def nombre(self):
        return self.__nombre

    @property
    def sueldo_base(self):
        return self.__sueldo_base

    @sueldo_base.setter
    def sueldo_base(self, nuevo_sueldo):
        if nuevo_sueldo >= 0:
            self.__sueldo_base = nuevo_sueldo
        else:
            raise ValueError("El sueldo no puede ser negativo.")

    def calcular_salario(self):
        return self.__sueldo_base

# Clase hija 1 - Empleado Fijo
class EmpleadoFijo(Empleado):
    def calcular_salario(self):
        # Sueldo base + bono fijo
        bono = 500
        return self.sueldo_base + bono

# Clase hija 2 - Empleado por Horas
class EmpleadoPorHoras(Empleado):
    def _init_(self, nombre, sueldo_base, horas_trabajadas):
        super()._init_(nombre, sueldo_base)
        self.horas_trabajadas = horas_trabajadas

    def calcular_salario(self):
        return self.sueldo_base * self.horas_trabajadas

# Clase hija 3 - Empleado Temporal
class EmpleadoTemporal(Empleado):
    def calcular_salario(self):
        # Gana el 80% del sueldo base
        return self.sueldo_base * 0.8

# PRUEBAS DE POLIMORFISMO
print("=== Cálculo de salarios con polimorfismo ===")
empleados = [
    EmpleadoFijo("Ana", 1500),
    EmpleadoPorHoras("Luis", 15, 120),
    EmpleadoTemporal("María", 2000)
]

for emp in empleados:
    print(f"{emp.nombre} gana: ${emp.calcular_salario():.2f}")
*
[8:11 p. m., 21/4/2025] Conde: 

