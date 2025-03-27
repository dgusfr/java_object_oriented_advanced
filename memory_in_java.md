# Alocação Dinâmica de Memória em Java

## Sumário

1. [Introdução](#1-introdução)  
2. [Tipos de Dados em Java](#2-tipos-de-dados-em-java)  
   - [2.1 Classes como Tipos Referência](#21-classes-como-tipos-referência)  
   - [2.2 Valor null](#22-valor-null)  
   - [2.3 Tipos Primitivos (Tipos Valor)](#23-tipos-primitivos-tipos-valor)  
   - [2.4 Valores Padrão](#24-valores-padrão)  
   - [2.5 Tabela Comparativa](#25-tabela-comparativa)  
3. [Resumo: Alocação e Acesso](#3-resumo-alocação-e-acesso)  
4. [Desalocação de Memória em Java](#4-desalocação-de-memória-em-java)  
   - [4.1 Garbage Collector](#41-garbage-collector)  
   - [4.2 Desalocação por Escopo Local](#42-desalocação-por-escopo-local)  
   - [4.3 Combinação GC + Escopo Local](#43-combinação-gc--escopo-local)  
5. [Conclusão](#5-conclusão)

---

## 1. Introdução

Em Java, a **alocação dinâmica de memória** é gerenciada automaticamente pela **JVM (Java Virtual Machine)**. Quando um objeto é criado por meio da palavra-chave `new`, a memória necessária para armazenar esse objeto é alocada na **Heap**, enquanto as variáveis de referência (que “apontam” para esses objetos) ficam na **Stack**.

### Por que isso é importante?
- **Performance:** Compreender como a memória é gerenciada ajuda a evitar gargalos de desempenho.  
- **Segurança e Robustez:** Evita vazamento de memória (memory leaks), pois o Garbage Collector libera objetos não mais utilizados.  
- **Manutenção:** Facilita o rastreamento de referências, evitando erros como `NullPointerException`.

---

## 2. Tipos de Dados em Java

Em Java, os dados são divididos em **tipos referência** (classes, arrays, interfaces, etc.) e **tipos valor** (primitivos como `int`, `double`, `boolean`, etc.). A forma como esses tipos são armazenados e manipulados difere significativamente.

### 2.1 Classes como Tipos Referência

Variáveis do tipo referência **não armazenam o objeto em si**, mas sim um **endereço** que aponta para o local do objeto na **Heap**. Podemos comparar essas variáveis a “tentáculos” que se estendem até o objeto.

#### Exemplo

```java
class Product {
    String name;
    double price;
    int quantity;

    Product(String name, double price, int quantity) {
        this.name = name;
        this.price = price;
        this.quantity = quantity;
    }
}

public class TestReferencias {
    public static void main(String[] args) {
        Product p1, p2;                       // (1)
        p1 = new Product("TV", 900.00, 0);    // (2)
        p2 = p1;                              // (3)

        p2.name = "Radio";

        System.out.println(p1.name); // Imprime: Radio
        System.out.println(p2.name); // Imprime: Radio
    }
}
```

**Explicação:**
1. `Product p1, p2;` declara duas variáveis de referência do tipo `Product`.
2. `p1 = new Product(...)` cria um novo objeto `Product` na **Heap** e faz `p1` apontar para ele.
3. `p2 = p1;` faz `p2` apontar para o **mesmo** objeto que `p1`. Assim, mudanças em `p2` também se refletem em `p1`.

<img src="images/memory.png" alt="alt" width="900">

---

### 2.2 Valor `null`

Tipos referência podem assumir o valor `null`, que indica que a variável **não aponta para nenhum objeto**. Tentar acessar métodos ou atributos de um objeto através de uma referência `null` resultará em `NullPointerException`.

```java
Product p1;
p1 = null;
System.out.println(p1.name); // Erro: NullPointerException
```
<img src="images/memory2.png" alt="alt" width="900">

---

### 2.3 Tipos Primitivos (Tipos Valor)

Tipos primitivos em Java (`int`, `double`, `boolean`, `char`, etc.) são **tipos valor**. Isso significa que a variável **armazena o valor em si**, diretamente na **Stack**, sem a necessidade de referências.

#### Exemplo

```java
double x, y;
x = 10;
y = x;  // y recebe uma cópia do valor de x
y = 20;

System.out.println(x); // 10
System.out.println(y); // 20
```

**Explicação:**  
- `y = x;` copia o valor de `x` para `y`. Quando `y` é alterado, `x` permanece o mesmo, pois cada variável contém **seu próprio valor**.

---

### 2.4 Valores Padrão

#### Tipos Primitivos

- **int, double, float, long, short, byte:** 0  
- **boolean:** false  
- **char:** caractere com código 0

#### Referências (Strings, Objetos, Arrays)

- **null**

> **Observação:** Variáveis **locais** em métodos exigem **inicialização explícita** antes do uso. Caso contrário, o compilador emitirá erro. Já atributos de instância (por exemplo, dentro de uma classe `Product`) recebem os valores padrão automaticamente quando a instância é criada com `new`.

---

### 2.5 Tabela Comparativa

| Característica               | Classe (Tipo Referência)                                              | Tipo Primitivo (Tipo Valor)                  |
|-----------------------------|------------------------------------------------------------------------|----------------------------------------------|
| **Armazenamento**           | Armazena **referências** na Stack; Objeto reside na Heap              | Armazena **valor** diretamente na Stack      |
| **Instanciação**            | Exige `new` (ou referências a objetos já existentes)                  | Não requer `new` (exceto em casos de wrappers) |
| **Aceita valor `null`**     | Sim                                                                   | Não                                          |
| **Atribuição**              | Copia a **referência** (apontam para o mesmo objeto)                  | Copia o **valor** (variáveis independentes)  |
| **Localização na Memória**  | **Heap** para o objeto; **Stack** apenas para a referência            | **Stack**                                    |
| **Desalocação**             | **Garbage Collector** (objeto é desalocado quando perde todas as referências) | Automática ao final do escopo               |
| **Vantagem**                | Recursos de POO (herança, polimorfismo, etc.)                         | Simples, rápido e direto ao ponto            |

---

## 3. Resumo: Alocação e Acesso

1. **Objetos** vivem na **Heap**.  
2. **Referências** (variáveis do tipo classe) vivem na **Stack** e apontam para objetos na Heap.  
3. **Tipos Primitivos** vivem diretamente na Stack, armazenando o valor em si.  
4. **null** representa uma referência que não aponta para objeto algum.  
5. A **memória** é gerenciada automaticamente pela JVM, mas conhecer esses detalhes evita erros e melhora a performance.

---

## 4. Desalocação de Memória em Java

Java gerencia a liberação de memória por dois mecanismos principais:

1. **Garbage Collector (GC)**  
2. **Desalocação por Escopo Local** (variáveis de método)

### 4.1 Garbage Collector

O **Garbage Collector (GC)** é um processo que identifica e remove objetos na Heap que **não possuem mais referências** apontando para eles.

- **Acompanhamento de Referências:** Se um objeto perde todas as referências, ele fica inacessível.
- **Exemplo Prático:**

```java
Product p1 = new Product("TV", 900.00, 0);
Product p2 = new Product("Mouse", 30.00, 0);

p1 = p2; // O objeto "TV" agora não é mais referenciado
```

**Antes:**
```
Stack          Heap
p1 ----------> ("TV", 900.00, 0)
p2 ----------> ("Mouse", 30.00, 0)
```

**Depois:**
```
Stack          Heap
p1 ----------> ("Mouse", 30.00, 0)
p2 ----------> ("Mouse", 30.00, 0)

(Nenhuma referência aponta para "TV", 900.00, 0) -> Coletável pelo GC
```

Quando o **GC** é acionado, o objeto “TV” é removido e a memória é liberada.

---

### 4.2 Desalocação por Escopo Local

As variáveis locais são armazenadas na **Stack** e desalocadas **automaticamente** quando o escopo onde foram declaradas é encerrado.

```java
void method1() {
    int x = 10;
    if (x > 0) {
        int y = 20;
        // 'y' só existe dentro deste bloco
    }
    System.out.println(x); // OK
    // System.out.println(y); // Erro: variável fora de escopo
}
```

<img src="images/memory3.png" alt="alt" width="900">

- `x` é desalocado ao final de `method1()`.
- `y` é desalocado assim que o bloco `if` é encerrado.

---

### 4.3 Combinação GC + Escopo Local

- **Variáveis Locais (Stack):** Desalocadas no final de cada bloco (escopo).
- **Objetos (Heap):** Removidos pelo Garbage Collector quando não há mais referências ativas.

#### Exemplo de Retorno de Objetos

```java
class Example {
    Product method2() {
        Product prod = new Product("Laptop", 2000.00, 1);
        return prod; 
    }

    void method1() {
        Product p = method2(); 
        // O objeto permanece na Heap apontado por 'p'
        // A variável local 'prod' de method2() já saiu do escopo
    }
}
```

1. `method2()` cria um objeto `Product` na Heap.
2. Retorna a referência para `method1()`, que armazena em `p`.
3. A variável local `prod` de `method2()` é desalocada ao final de `method2()`, mas o **objeto** persiste na Heap porque `p` ainda o referencia.

Memória Após a Execução de method1:

<img src="images/memory4.png" alt="alt" width="900">

---

## 5. Conclusão

Em Java, o gerenciamento de memória é **automático**, mas compreender como a **Stack**, a **Heap** e o **Garbage Collector** interagem é **essencial** para produzir código seguro, eficiente e de fácil manutenção.

- **Objetos** são criados na Heap e sobrevivem até que não haja mais referências.  
- **Variáveis locais** na Stack desaparecem ao final do escopo, mas **objetos** só são removidos pelo GC quando se tornam inacessíveis.  
- **Tipos Referência** versus **Tipos Primitivos**: entender essa diferença evita erros como `NullPointerException` e garante um uso mais consciente da memória.  
- **Valores Padrão** e **construtores** facilitam a inicialização de variáveis e objetos sem sobrescrever manualmente cada atributo.

Ao dominar esses conceitos, você aproveita melhor os recursos da POO em Java e escreve aplicações mais robustas.