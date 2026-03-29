## Avantages

Rapidité, langage facile à prendre en main, pas besoin de gérer la libération de mémoire
## Pointeurs
Une variable quel que soit le type (primitif ou complexe) est **passée par valeur**.
```go
func mutate(x int) {
  x = 10
}
func main() {
  x := 5
  fmt.Printf(x)
} 
```

## Formatage de String

### Les différentes méthodes de Print
- If the name **starts** with `Print`, it writes to standard output
- If the name **starts** with `Fprint`, it writes to an `io.Writer` (possibly to a file, thus the 'f')
- If the name **starts** with `Sprint`, it writes to a `string` and **returns** that `string`
- If the name **ends** with `f`, it is a formatted print, it gets a format argument like `"%s %d"`, and formats the output based on that.
- If the name **ends** with `ln` like `Println`, it prints a newline after writing
- Otherwise it simply prints its arguments using their default formats.

### Formatage
- `"%0Xd"` permet de faire du string padding and d'ajouter des 0 devant la valeur donnée jusqu'à obtenir une string de `X` longueur.
- `"%Xd"` fait la même chose que le format précédent, mais le remplissage se fera des espaces.

```go
import "fmt"

// returns 001
formattedString := fmt.Sprintf("%03d", 1)
```

## Conditions

En **Golang** il est possible de déclarer une variable directement dans un `IF`:
```go
if _, ok := map[key]; !ok {
	return false
}
```

revient à
```go
_, ok := map[key]
if !ok {
	return false
}
```
Cela est utile lorsque la variable n'est nécessaire que dans le scope du `IF`.
## Map

Pour vérifier si une valeur existe dans une map
```go
value, ok := map[key]
```

Si nous n'avons pas besoin d'utiliser `value`on peut l'ignore à l'aide du caractère `_`.
`_` est un "**identifiant vide**", elle est utilisé pour ignorer une valeur en **Golang**.

*exemple :*
```go
_, exists := myMap["key"]
```
- la valeur `myMap["key"]` est ignoré, car nous n'en avons pas besoin.
- `exists` indique si la clé existe.
Contrairement au **Javascript** dans une signature de fonction lorsqu'une valeur n'est pas utilisée on met un `_` devant le nom de la variable. Mais en **Golang** le caractère `_` ne peux pas être nommé. 
## Retour de valeurs

Comme vu précédemment avec la manipulation des maps, en **Golang** il est possible de retourner plusieurs valeurs:
```go
func getUser() (string, int) {
    return "Alice", 30
}

name, age := getUser()
```
Mais ce n'est pas de la déstructuration à proprement parler.

## Pointeurs

```go
var a int
a = 2

var p *int // pointeur d'un int
p = &a // the variable 'p' contains the memory address of 'a'

var b int
b = *p // b == 2

var pa *int
pa = &a          // 'pa' now contains to the memory address of 'a'
*pa = *pa + 2    // increment by 2 the value at memory address 'pa'

fmt.Println(a)   // Output: 4
                 // 'a' will have the new value that was changed via the pointer!


// Example on Stucts
type Person struct {
    Name string
    Age  int
}

var peter Person
peter = Person{Name: "Peter", Age: 22}

var p *Person
p = &peter

// a different way of creating a new Entity
var p *Person
p = &Person{Name: "Peter", Age: 22}
fmt.Println(p.Name) // Output: "Peter"
                    // Go automatically dereferences 'p' to allow
                    // access to the 'Name' field

```

- `&a`: contient l'adresse en mémoire de `a`
- `*p` : Déréférencer un pointeur revient **à accéder à la valeur stockée à l'adresse mémoire indiquée par ce pointeur** . Cela permet de travailler avec les données réelles plutôt qu'avec l'emplacement mémoire.

## Interface

```go
import "fmt"

type I interface {
	A()string
	B()
}

type T struct {
 S string
}

// cet méthode indique que T implément l'interface I, il n'y a pas de mot clé implements commen dans d'autres langages
func (t T) A()string {
	return "Hello"
}
func (t T) B() {
	fmt.Println(t.S)
}

func main() {
	i = T{ "Good night"}
	fmt.Println(i.A()); // Output: "Hello"
	i.B(); // Output: "Good night"
}
```