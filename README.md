#ENCAPSULAMIENTO

#Clase principal
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

###Este programa maneja la información de un empleado de forma privada y segura, y permite verificar y cambiar su clave de acceso de manera controlada##

