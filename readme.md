# **Blocks**
## **Métdos**

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

Los **simbolos** también peuden ser convertidos a Procs usando el útil *&*
```ruby
strings = ["1", "2", "3"]
nums = strings.map(&:to_i)
```
