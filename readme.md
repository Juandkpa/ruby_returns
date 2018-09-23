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
2. Cuando un lambda retorna, pasa el control al método desde el cual fue invocado, en cambio cuando retorna, lo hace de inmediato sin pasar por el método que lo ha invocado.

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
- ### **Varibles Globales:** Están disponibles en cualquier lugar.
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

## **Ejemplo:** un perro es un animal.
En ruby la herencia se define como:
```ruby
Perro < Mamífero
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





   
