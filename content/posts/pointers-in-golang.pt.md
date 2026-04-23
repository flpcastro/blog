---
title: "Ponteiros em Golang"
date: 2026-03-16
draft: false
tags: ["go", "concurrency", "backend"]
categories: ["golang"]
---

![Golang](/images/go.webp)

E aí, Gopher, tudo certo?

Ultimamente venho dedicando meu tempo livre ao estudo de Golang e preciso admitir: tem sido uma jornada interessante e desafiadora.

Durante o processo de aprendizado, tive inicialmente uma certa “resistência” para entender o que de fato são ponteiros. Por coincidência, acompanhando alguns blogs e páginas sobre Go, percebi que muita gente passa pelo mesmo obstáculo — e isso é completamente normal.

Foi por isso que resolvi criar este espaço para escrever um pouco sobre eles.

## Afinal, o que é um ponteiro?

Um ponteiro é simplesmente uma variável cujo conteúdo é um endereço de memória. Se você compartilhar esse ponteiro em outro lugar, conseguimos acessar diretamente o endereço em que o dado está armazenado e, a partir daí, manipular de alguma forma aquele espaço de memória.

### Um resumo rápido de como a memória funciona:

![Endereços](/images/pointers-addresses.webp)

Em resumo, a memória é composta por endereços que armazenam dados, chamados de “valores” na imagem acima. Cada variável criada no código recebe um endereço de memória, e o valor correspondente é armazenado nesse endereço.

```go
package main

func main() {
  a := 15
  b := "foo"
}
```
> No trecho de código acima, duas variáveis foram declaradas: uma chamada `a` com o valor 15, e outra chamada `b` com o valor "foo".

![Endereços](/images/pointers-addresses-2.webp)
> A imagem acima mostra o endereço de memória atribuído a cada variável e seus respectivos valores.

## Como declarar um ponteiro?
Um ponteiro é declarado usando `*` seguido do tipo de dado que se deseja declarar, como **int**, **string**, etc.

```go
package main

func main() {
  var p *int

  /* Aqui, o ponteiro foi declarado,
  mas nenhum endereço de memória foi atribuído a ele. */
}
```

## Como atribuir um endereço de memória a um ponteiro?
Atribuímos o endereço de uma variável ao ponteiro usando `&` antes da variável (ex.: **&a**).

```go
package main

import "fmt"

func main() {
  var p *int

  a := 15 // declarando uma variável do tipo int
  p = &a // pegando o endereço da variável (a)
        // e atribuindo ao ponteiro (*)

  fmt.Println(p) // OUTPUT: endereço de memória atribuído, exemplo:
                // 0x140000140a5
}
```

## Como saber o valor armazenado no endereço de memória?
Para obter o valor armazenado no endereço de memória atribuído ao ponteiro, basta usar `*` antes do ponteiro (ex.: **p**).

```go
package main

import "fmt"

func main() {
  var p *int

  a := 15
  p = &a

  fmt.Println(*p) // OUTPUT: 15 -> Valor armazenado no endereço.
}
```

## Quando usar ponteiros?
É sabido que ponteiros ajudam quando o assunto é performance. Mas quando devemos usá-los?

### Mutabilidade
A única maneira de modificar uma variável passada para uma função é passando um `ponteiro`. Por padrão, passar por valor significa que qualquer alteração é feita em uma **cópia** com a qual você está trabalhando. Como resultado, essas alterações não são refletidas na função chamadora. **Veja um exemplo**:

```go
package main

import "fmt"

type dog struct {
  name string
}

func main() {
  d := dog{"Fifo"}
  changeDogName(d)
  fmt.Println(d)
}

func changeDogName(d dog) {
  d.name = "Mike"
}
```
> A saída do **fmt.Println** será “**Fifo**” e não “**Mike**”, porque o nome do cachorro está sendo alterado em uma “cópia” da variável. Para corrigir isso, basta usar um ponteiro.


```go
package main

import "fmt"

type dog struct {
  name string
}

func main() {
  d := dog{"Fifo"}
  changeDogName(&d)
  fmt.Println(d)
}

func changeDogName(d *dog) {
  d.name = "Mike"
}
```
> Agora a saída será “**Mike**” 🙂

Existem outros casos em que o uso de ponteiros é recomendado, mas, na minha opinião, mutabilidade é o principal. Você encontra outros casos em um ótimo artigo que usei como referência nos meus estudos.

## Enfim, é isso…
Só um lembrete: a ideia de escrever esse breve resumo sobre ponteiros surgiu principalmente para fins de aprendizado e, quem sabe, ajudar alguém com dúvidas sobre o assunto.

Usei este excelente [artigo](https://medium.com/@meeusdylan/when-to-use-pointers-in-go-44c15fe04eac) do Dylan Meeus como referência — recomendo muito a leitura. 😁
