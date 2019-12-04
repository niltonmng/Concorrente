# Concorrente

## Goroutines
Uma goroutine é um segmento leve e gerenciado pelo runtime de Go.
```
go f(x, y, z)
```
Goroutines executam no mesmo espaço de endereço, para que o acesso à memória compartilhada seja sincronizada. O pacote **sync** fornece as primitivas úteis.

## Canais:
Canais são um conduto tipado através do qual você pode enviar e receber valores com o operador de canal, **<-**.
  Em Go, o canal armazena os valores de entrada, a exemplo abaixo:
```
ch <- v    // v envia para o canal ch.
v := <-ch  // Recebe do ch, e
           // atribui o valor de v.
```
  (Os dados fluem na direcao da seta.)
  
  Devem ser criados antes de usar
```
    ch := make(chan int)
```
Por padrão, enviam e recebem bloco até o outro lado estar pronto. Isso permite que goroutines sincronizem sem bloqueios explícitos ou variáveis de condição.

*obs.: Acredito que o canal seja uma fila, onde a informações saem por ordem de chegada, ou seja, se um valor x é atribuido a um canal, e em seguida um valor y é atribuido ao mesmo canal, ao*

### Canais Bufferizados

Os canais podem ser bufferizados. Fornecendo o tamanho do buffer como o segundo argumento para make para inicializar um canal bufferizado:
```
ch := make(chan int, 100)
```
Envia para um bloco de canais bufferizados apenas quando o buffer está cheio. Recebe bloco quando o buffer está vazio.
