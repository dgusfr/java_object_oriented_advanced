Segue abaixo os **exercícios sobre Interfaces** reformulados, com **enunciados claros** e **palavras-chave em negrito**. O leitor deve **interpretar** o que é pedido, mas todas as informações para resolver cada exercício estão presentes:

---

### **Exercício 1 – Sintaxe de Interfaces e Classes que as Implementam**

Considere a seguinte interface:

```java
interface AreaCalculavel {
    double calculaArea();
}
```

Crie classes como **`Quadrado`**, **`Circulo`** e **`Retangulo`** que **implementam** essa interface. Por exemplo, cada figura deve **oferecer** o método `calculaArea()` para retornar a **área correspondente**:

```java
class Quadrado implements AreaCalculavel {
    private int lado;

    Quadrado(int lado) {
        this.lado = lado;
    }

    public double calculaArea() {
        // Sua fórmula para calcular a área
        return this.lado * this.lado;
    }
}

// Faça o mesmo para Circulo e Retangulo
```

Analise por que **herança** não seria apropriada nesse caso, e como as **interfaces** ajudam a manter as classes **desacopladas**, além de permitir que cada classe tenha sua própria lógica de cálculo.

---

### **Exercício 2 – Sistema de Tributação**

Crie uma interface chamada:

```java
interface Tributavel {
    double calculaTributos(double taxa);
}
```

Agora, identifique quais **bens ou classes** do seu sistema devem **implementar** essa interface. Por exemplo, algumas contas (como `ContaCorrente`) e também `SeguroDeVida` podem gerar tributos:

```java
class ContaCorrente extends Conta implements Tributavel {
    // Implementar calculaTributos(double taxa)
    // Retornar 0.1 * taxa * saldo (exemplo)
}

class SeguroDeVida implements Tributavel {
    // Implementar calculaTributos(double taxa)
    // Somar a taxa ao saldo, acrescido de 10 reais (exemplo)
}
```

Em seguida, crie um **GerenciadorDeImpostoDeRenda** que receba objetos `Tributavel` e some seus valores de tributo:

```java
class GerenciadorDeImpostoDeRenda {
    private double taxa;
    private double total;

    GerenciadorDeImpostoDeRenda(double taxa) {
        this.taxa = taxa;
    }

    void adiciona(Tributavel t) {
        System.out.println("Adicionando tributável: " + t);
        // Some aqui o resultado de t.calculaTributos(taxa) ao atributo 'total'
        // e imprima o valor atualizado de 'total'
    }
}
```

Por fim, crie um `main` que **instancie** diversos objetos que implementem `Tributavel` (por exemplo, algumas contas correntes e seguros de vida) e **passe-os** ao `GerenciadorDeImpostoDeRenda`. Note que apenas os objetos que **implementarem** `Tributavel` poderão ser adicionados ao gerenciador, reforçando o **polimorfismo** via interface.

---

