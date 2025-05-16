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
