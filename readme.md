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
