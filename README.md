# Tercera tarea de APA: Multiplicaciones de vectores y ortogonalidad

## Nom i cognoms : Montserrat Cuevas López

El fichero `algebra/vectores.py` incluye la definición de la clase `Vector` con los
métodos desarrollados en clase, que incluyen la construcción, representación y
adición de vectores, entre otros.

Añada a este fichero los métodos siguientes, junto con sus correspondientes
tests unitarios.

### Multiplicación de los elementos de dos vectores (Hadamard) o de un vector por un escalar

- Sobrecargue el operador asterisco (`*`, correspondiente a los métodos `__mul__()`,
  `__rmul__()`, etc.) para implementar el producto de Hadamard (vector formado por
  la multiplicación elemento a elemento de dos vectores) o la multiplicación de un
  vector por un escalar.

  - La prueba unitaria consistirá en comprobar que, dados `v1 = Vector([1, 2, 3])` y
    `v2 = Vector([4, 5, 6])`, la multiplicación de `v1` por `2` es `Vector([2, 4, 6])`,
    y el producto de Hadamard de `v1` por `v2` es `Vector([4, 10, 18])`.

- Sobrecargue el operador arroba (`@`, multiplicación matricial, correspondiente a los
  métodos `__matmul__()`, `__rmatmul__()`, etc.) para implementar el producto escalar
  de dos vectores.

  - La prueba unitaria consistirá en comprobar que el producto escalar de los dos
    vectores `v1` y `v2` del apartado anterior es igual a `32`.

### Obtención de las componentes normal y paralela de un vector respecto a otro

Dados dos vectores $v_1$ y $v_2$, es posible descomponer $v_1$ en dos componentes,
$v_1 = v_1^\parallel + v_1^\perp$ tales que $v_1^\parallel$ es tangencial (paralela) a
$v_2$, y $v_1^\perp$ es normal (perpendicular) a $v_2$.

> Se puede demostrar:
>
> - $v_1^\parallel = \frac{v_1\cdot v_2}{\left|v_2\right|^2} v_2$
> - $v_1^\perp = v_1 - v_1^\parallel$

- Sobrecargue el operador doble barra inclinada (`//`, métodos `__floordiv__()`,
  `__rfloordiv__()`, etc.) para que devuelva la componente tangencial $v_1^\parallel$.

- Sobrecargue el operador tanto por ciento (`%`, métodos `__mod__()`, `__rmod__()`, etc.)
  para que devuelva la componente normal $v_1^\perp$.

> Es discutible esta elección de las sobrecargas, dado que extraer la componente
> tangencial no es equivalente a ningún tipo de división. Sin embargo, está
> justificado en el hecho de que su representación matemática es dos barras
> paralelas ($\parallel$), similares a las usadas para la división entera (`//`).
>
> Por otro lado, y de manera *parecida* (aunque no idéntica) al caso de la división
> entera, las dos componentes cumplen: `v1 = v1 // v2 + v1 % v2`, lo cual justifica
> el empleo del tanto por ciento para la componente normal.

- En este caso, las pruebas unitarias consistirán en comprobar que, dados los vectores
  `v1 = Vector([2, 1, 2])` y `v2 = Vector([0.5, 1, 0.5])`, la componente de `v1` paralela
  a `v2` es `Vector([1.0, 2.0, 1.0])`, y la componente perpendicular es `Vector([1.0, -1.0, 1.0])`.

### Entrega

#### Fichero `algebra/vectores.py`

- El fichero debe incluir una cadena de documentación que incluirá el nombre del alumno
  y los tests unitarios de las funciones incluidas.

- Cada función deberá incluir su propia cadena de documentación que indicará el cometido
  de la función, los argumentos de la misma y la salida proporcionada.

- Se valorará lo pythónico de la solución; en concreto, su claridad y sencillez, y el
  uso de los estándares marcados por PEP-ocho.

#### Ejecución de los tests unitarios

Inserte a continuación una captura de pantalla que muestre el resultado de ejecutar el
fichero `algebra/vectores.py` con la opción *verbosa*, de manera que se muestre el
resultado de la ejecución de los tests unitarios.

![alt text](image.png)

#### Código desarrollado

Inserte a continuación el código de los métodos desarrollados en esta tarea, usando los
comandos necesarios para que se realice el realce sintáctico en Python del mismo (no
vale insertar una imagen o una captura de pantalla, debe hacerse en formato *markdown*).

```python

"""
    Tercera tarea de APA - manejo de vectores

    Nombre y apellidos: Montserrat Cuevas López
"""

class Vector:
    """
    Clase usada para trabajar con vectores sencillos
    """
    def __init__(self, iterable):
        """
        Costructor de la clase Vector. Su único argumento es un iterable con las componentes del vector.
        """

        self.vector = [valor for valor in iterable]

        return None      # Orden superflua

    def __repr__(self):
        """
        Representación *oficial* del vector que permite construir uno nuevo idéntico mediante corta-y-pega.
        """

        return 'Vector(' + repr(self.vector) + ')'

    def __str__(self):
        """
        Representación *bonita* del vector.
        """

        return str(self.vector)

    def __getitem__(self, key):
        """
        Devuelve un elemento o una loncha del vector.
        """

        return self.vector[key]

    def __setitem__(self, key, value):
        """
        Fija el valor de una componente o loncha del vector.
        """

        self.vector[key] = value

    def __len__(self):
        """
        Devuelve la longitud del vector.
        """

        return len(self.vector)

    def __add__(self, other):
        """
        Suma al vector otro vector o una constante.
        """

        if isinstance(other, (int, float, complex)):
            return Vector(uno + other for uno in self)
        else:
            return Vector(uno + otro for uno, otro in zip(self, other))

    __radd__ = __add__

    def __neg__(self):
        """
        Invierte el signo del vector.
        """

        return Vector([-1 * item for item in self])

    def __sub__(self, other):
        """
        Resta al vector otro vector o una constante.
        """

        return -(-self + other)

    def __rsub__(self, other):     # No puede ser __rsub__ = __sub__
        """
        Método reflejado de la resta, usado cuando el primer elemento no pertenece a la clase Vector.
        """

        return -self + other
    
    def  __mul__(self, other):
        """
        Método para dar un vector formado por la multiplicación elemento a elemento de dos vectores o multiplicar un vector por un número

        >>> v1 = Vector([1, 2, 3])
        >>> v2 = Vector([4, 5, 6])

        >>> v1 * v2
        Vector([4, 10, 18])

   
        >>> v1 * 2
        Vector([2, 4, 6])

        """

        if isinstance(other,(int, float, complex)):
            return Vector(item * other for item in self)
        else: 
            return Vector(num1 * num2 for num1, num2 in zip(self, other))
        
    __rmul__ = __mul__
    # igualamos __rmul__ a __mul__ porque la multiplicación es commutativa. 

    def __matmul__(self, other):

        """
        Método para multiplicar matrices 

        >>> v1 = Vector([1, 2, 3])
        >>> v2 = Vector([4, 5, 6])

        >>> v1 @ v2
        32
        """
        if isinstance(other, Vector):
            return sum(num1 * num2 for num1, num2 in zip(self, other))
        else:
            raise TypeError("No se puede realizar la operación porque no es un vector")
    
    def __rmatmul__(self, other):
        """
        Método para comprovar que en la otra posición, el otro elemento también es un vector, si lo es usará el método __mul__

        """
        if isinstance(other, Vector):
            return other @ self #reutiliza __mul__
        else:
            raise TypeError("No se puede realizar la operación porque no es un vector")
        
    def __floordiv__(self, other):

        """
        Método que devuleve la componente v1 paralela a v2

        >>> v1 = Vector([2, 1, 2])
        >>> v2 = Vector([0.5, 1, 0.5])

        >>> v1 // v2
        Vector([1.0, 2.0, 1.0])
        """

        if isinstance(other, Vector):
            factor = ( self @ other )/(other @ other)
            return Vector(item * factor for item in other)
        else:
            raise TypeError("No se puede realizar la operación porque no es un vector")
        
    def __rfloordiv__(self, other):
        """
        Método para comprovar que el otro componente (v2) es un vector con el reflejado de __floordiv__
        """
        if isinstance(other, Vector):
            return self // other #reutiliza __floordiv__
        else:
            raise TypeError("No se puede realizar la operación porque no es un vector")
    
        
    def __mod__(self, other):
        """
        Método para calcular la componente normal de un vector (v1) respecto a otro vector(v2)

        >>> v1 = Vector([2, 1, 2])
        >>> v2 = Vector([0.5, 1, 0.5])

        >>> v1 % v2
        Vector([1.0, -1.0, 1.0])
        """
        if isinstance(other, Vector):
            return Vector([num1 - num2 for num1, num2 in zip(self, self // other)])
        else:
            raise TypeError("No se puede realizar la operación porque no es un vector")
        
    def __rmod__(self, other):
        """
        Método para comprovar que en el reflejado de __mod__ el otro componente es un vector
        """
        if isinstance(other, Vector):
            return self % other #reutiliza __mod__
 
 
if __name__ == "__main__":
      import doctest
      doctest.testmod()
    
```

#### Subida del resultado al repositorio GitHub y *pull-request*

La entrega se formalizará mediante *pull request* al repositorio de la tarea.

El fichero `README.md` deberá respetar las reglas de los ficheros Markdown y
visualizarse correctamente en el repositorio, incluyendo la imagen con la ejecución de
los tests unitarios y el realce sintáctico del código fuente insertado.
