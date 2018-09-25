# **Blocks**
## **Métodos**

### **Collect**: Toma un block y aplica la expresíón a cada elemento en una array
```ruby
my_nums = [3, 4, 5, 8]
my_nums.collect! { |num|
    num ** 2
}
```

### **Yield**: Algunos métodos aceptan blocks porque a través del yield transfieren el control desde el método hacia el bloque y viceversa

#### Yieling con parametros
```ruby
def yield_name(name)
    puts "En el método, vamos a usar yield"
    yield("kim")
    puts "En medio de yields"
    yield(name)
    puts "Block completado, de nuevo en el metodo"
end

yield_name("juan") { |n|
    puts "Mi nombre es #{n}!"
}
```
**No todo son objetos en ruby, los blocks, no son objetos.

# **Procs**
Son blocks que se pueden guardar. Son propicios para la mantenibilidad del código y cumplir con el principio **DRY** (Not Repeat yourself)

## **Sintaxis:** 
Son faciles de definir, simplemente se debe pasar el block que se quiere guardar a *Proc.new*
```ruby
cube = Proc.new {|x| x**3 }
```
luego se puede pasar a un método que de otra manera recibiría un block y así no tenemos que escribir el block una y otra vez.
```ruby
[1,2,3].collect! (&cube)
[4,5,6].collect! (&cube)
```

& sirve para convertir el Proc en block, pues estos metodos normalmente reciben blocks, esto se hace cuando un método espera un block.

## **¿Por qué Procs?**
- Son objetos, tienen todos los beneficios de un objeto
- Evitan **DRY**
- third

```ruby
def greeter
    yield
end

phrase = Proc.new {puts "Hola a todos!"}
greeter(&phrase)
```
Los Procs pueden ser llamados directamente usando el método de Ruby .call.

```ruby
hi = Proc.new { puts "Hello"}
hi.call
```

Los **símbolos** también peuden ser convertidos a Procs usando el útil *&*
```ruby
strings = ["1", "2", "3"]
nums = strings.map(&:to_i)
```

# **Lamba**
Son muy similares a los procs.

```ruby
lambda { puts "hello" }
Proc.new { puts "hello"}

def lambda_demo(a_lambda)
    puts "Estoy en el método!"
    a_lambda.call
end

lambda_demo( lambda {puts "Yo soy el lambda"} )
```
## **Sintaxis:** 
```ruby
lambda { |param| block}
```
##  **Ejemplo:**
```ruby
symbolize = lambda { |st| st.intern }
strings.collect!(&symbolize)
```


# **Lambda Vs Procs**
Como habiamos dicho antes son muy similares, solo tienen 2 diferencias:

1. Lambda verifica el número de argumentos que se le pasan, Así si se le pasan un número erroneo de argumentos, lanzará un error. Cosa que no pasa en el Proc, que simplemente lo ignora asignandole *nil*
2. Cuando un lambda retorna, pasa el control al método desde el cual fue invocado, en cambio cuando el Procs retorna, lo hace de inmediato sin pasar por el método que lo ha invocado.

```ruby
my_arr = ["raindrops", :kettless, "Whiskers", :mittens, :package]
symbol_filter = lambda{ |p| p.is_a? Symbol }
symbols = my_arr.select(&symbol_filter)
```

# **Clases**
## **Sintaxis**:
```ruby
class NewClass
    #code here
end
```
## **Convesión:** 
Empieza con mayuscula y usa camelcase
### **Constructor**
```ruby
def initialize()
end
```
### **Variable de instancia**
```ruby
@name
```
Un nuevo objeto se crea como :
```ruby
matz = Person.new("Yukihiro")
```

# **Alcance (Scope)**
El scope de una variable es el contexto en el cual esta es visible para el programa.

No todas las variables son accesibles en todos las partes de un programa todo el tiempo.

## **Exiten:**
- ### **Varibles Globales:**  Están disponibles en cualquier lugar.
- ### **Variables Locales:** Están disponibles dentro de la instancia de una clase.
- ### **Variables de clase:** Están disponibles solo en la clase en particular.

** la misma logica se tiene para los métodos.
```ruby
$global #variable global
@instancia #variable local o de instancia
@@class_var  #variable de clase
```
# **Herencia**
Es usada para expresar una relación de un a es un b. Una clase toma metodos y atributos de otra

## **Ejemplo:** un perro es un mamífero.
En ruby la herencia se define como:
```ruby
Perro < Mamifero
```
```ruby
class Mamifero
    def behave
        puts "Me comporto como mamifero"
    end
end

class Perro < Mamifero
end

tony = Perro.new
tony.behave
```

## **Sobreescribir (Override)**
Es posible sobreescribir un método de la clase padre en su clas ehija, para añadir el compartamiento deseado.
```ruby 
class Creature
    def initialize(name)
        @name = name
    end

    def fight
        return "Puch to the chops!"
    end
end

class Dragon < Creature
    def fight
        return "Breathes fire!"
    end
end
```


## **Super**
Se usa para llamar al método de la clase padre, desde la clase hija.

```ruby
class Dragon < Creature
    def fight
        puts "Instead of breathing fire .. "
        super()
    end
end
```
Lo que hace es buscar el método que tenga el mismo nombre en la clase padre y ejecutarlo. Deben pasarse los argumentos si estos son necesarios.

**Truco:** Si se quiere finalizar una declaración ruby sin necesidad de crear una nueva linea, se puede usar **;**
```ruby
class Monkey
end
class Monkey ; end
```
## **Ejemplo**
```ruby
class Message
    @@messages_sent = 0
    def initialize(from, to)
        @from = from
        @to   = to
        @@messages_sent += 1
    end
end

class Email < Message
    def initialize(from, to)
        super(from, to)
    end
end
```

# **Métodos**
## **De clase**
Métodos que manejan variables de clase y que corren sobre la clase y no la instancia, se definen como
```ruby
def Computer.get_user()
    return @@users
end
```

## **Públicos y Privados**
```ruby
class Person
    def initialize(name, age)
        @name = name
        @age = age
    end

    public
    def about_me
        puts "Soy #{@name} y tengo #{@age} años"
    end

    private
    def bank_account_number
        @account_number = 12345
        puts "Mi número de cuenta bancaria es #{@account_number}"
    end
end

eric = Person.new("Eric", 26)
eric.about_me
eric.bank_account_number
```
*En ruby los métodos son públicos por defecto.

## **attr_reader, attr_writer**
Sirven para poder leer o escribir un atributo de la clase, desde una instancía de la misma
```ruby
class Person
    attr_reader :name
    attr_writer :name
    def initialize(name)
        @name=name
    end
end
```
## **attr_accessor**
Crea las dos posibilidades.
```ruby
class Person
    attr_reader :name
    attr_accessor :job
    def initialize(name, job)
        @name = name
        @job = job
    end
end
```

# **Módulos**

Un módulo es como una caja de herramientas que contiene un conjunto de métodos y constantes que usan en las ocaciones que se necesiten.

```ruby
module Circle
    PI = 3.141592653589793
    
    def Circle.area(radius)
        PI * radius ** 2
    end

    def Circle.circumferencia(radius)
        2 * PI * radius
    end
end
```
*Ruby no restringe cuando vamos a reasignar el valor de una constante, pero arroja una advertencia.

Uno de los principlaes propositos de los módulos es separar las constantes y métodos dentro de espacios de nombres namespaces, de esta manera Ruby no confunte Math::PI vs Circle::PI

*:: Scope Resolution Operator*
## **Módulos Existentes**
Algunos módulos se cargan por defecto en el navegador, como el módulo Math, otros por el contrario debemos llamarlos de forma explicita, así:
```ruby
require 'module'

require 'date'
puts Date.today
```
Además de usar require, también se puede usar include, en este caso la clase que incluye un módulo puede usar todos sus métodos directamente, sin usar el *:: scope resolution operator*

```ruby
class Angle
    include Math
    attr_accessor :radians
    
    def initialize(radias)
        @radias = radians
    end

    def cosine
        cos(@radians)
    end
end

acute = Angle.new(n)
acute.cosine
```

## **Relación entre módulos y clases**
## **Mixin**
Es cuando un módulo es usado para adicionar comportamiento e información de ntro de una clase (instancia)
Nos permite personalizar una clase sin tener que reescribir código.
```ruby
module Action
    def jump
        @distance = rand(4) + 2
        puts " I jumped forward #{@distance} feet!"
    end
end

class Rabbit
    include Action
    attr_reader :name
    def initialize(name)
        @name = name
    end
end

class Cricket
    include Action
    attr_reader :name
    def initialize(name)
        @name = name
    end
end

peter = Rabit.new("Peter")
jim = Cricket.new("Jim")

peter.jump
jim.jump
```

## **Imitando múltiple herencia**
De esta manera se puede añadir comportamientos de varios módulos en una definición de clase.

## **Extend**
Es usada para agregar comportamientos y caracteristicas de un módulo a una clase, a nivel de clase.

```ruby
module ThePresent
    def now
        puts "It's #{Time.new.hour}"
    end
end

class ThehereAnd
    extend ThePresent
end

ThehereAnd.now
```









   
