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