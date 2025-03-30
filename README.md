Em sistemas orientados a objetos, às vezes criamos classes apenas para **organizar código comum** e **habilitar o polimorfismo**, como `Funcionario`, que serve de base para `Gerente`, `Diretor` etc. A classe `Funcionario` pode ter atributos como `nome`, `cpf` e `salario`, além de métodos como `getBonificacao()`.

No exemplo abaixo, usamos uma referência do tipo `Funcionario` para registrar bonificações de diferentes cargos:

```java
ControleDeBonificacoes cdb = new ControleDeBonificacoes();
Funcionario f = new Funcionario(); // Isso faz sentido?
cdb.registra(f);
```

Aqui surge o problema: **não faz sentido criar um objeto que é apenas um "Funcionario"**, já que esse conceito é genérico demais. Faz sentido usar uma **referência** do tipo `Funcionario`, mas o **objeto real** precisa ser algo concreto, como um `Gerente`.

Para resolver isso, usamos **classes abstratas**, definidas com a palavra-chave `abstract`. Elas **não podem ser instanciadas diretamente**:

```java
public abstract class Funcionario {
    protected String nome;
    protected String cpf;
    protected double salario;

    public abstract double getBonificacao(); // obrigatória nas subclasses
}
```

Isso impede o uso de `new Funcionario()` e obriga cada subclasse a implementar seu próprio cálculo de bonificação. Também usamos esse recurso quando queremos garantir que uma `Pessoa` seja sempre `PessoaFisica` ou `PessoaJuridica`, por exemplo.

Em resumo: usamos classes abstratas quando queremos criar **modelos genéricos** que **não devem existir sozinhos**, mas sim servir como base para classes mais específicas.

Aqui está a explicação direta e clara sobre **classes abstratas** e **métodos abstratos**:

---

### **Classe Abstrata**

Uma **classe abstrata** é aquela que não pode ser instanciada diretamente. Ela funciona como um **modelo ou rascunho** para suas subclasses. Por exemplo, a classe `Funcionario` é um conceito geral. Não queremos criar objetos que sejam apenas do tipo `Funcionario`, mas sim tipos concretos, como `Gerente` ou `Diretor`.

Para impedir que alguém crie um objeto genérico, utilizamos a palavra-chave **`abstract`** na definição da classe:

```java
abstract class Funcionario {
    protected double salario;

    public double getBonificacao() {
        return this.salario * 1.2;
    }
}
```

O seguinte código, por exemplo, não compila:

```java
Funcionario f = new Funcionario(); // erro de compilação!
```

**Por que usar classe abstrata?**  
Porque ela garante que apenas objetos de subclasses concretas possam ser criados, garantindo consistência no sistema.

---

### **Método Abstrato**

Um **método abstrato** é aquele que não tem implementação na classe abstrata, apenas **obriga as subclasses** a implementá-lo. Ele é declarado com a palavra-chave **`abstract`** e sem corpo (apenas ponto e vírgula):

```java
abstract class Funcionario {
    protected double salario;

    public abstract double getBonificacao();
}
```

- Ao declarar um método abstrato, todas as subclasses **devem obrigatoriamente** implementar esse método.
- Se uma subclasse não implementar, ocorrerá um **erro de compilação**.

Exemplo de subclasse implementando o método abstrato:

```java
class Gerente extends Funcionario {
    public double getBonificacao() {
        return this.salario * 1.4 + 1000;
    }
}
```

Dessa forma, o método abstrato garante que cada tipo específico implemente sua própria lógica, como a bonificação.

---

### **Outro exemplo: Classe Conta**

Suponha que existam tipos diferentes de contas bancárias que precisam ser atualizadas diariamente, como `ContaCorrente` e `ContaPoupanca`. Cada conta se atualiza de forma diferente, então faz sentido tornar o método `atualiza` abstrato para forçar cada conta a definir sua própria lógica.

Classe abstrata `Conta`:

```java
abstract class Conta {
    protected double saldo;

    public void deposita(double valor) {
        this.saldo += valor;
    }

    public void retira(double valor) {
        this.saldo -= valor;
    }

    public double getSaldo() {
        return this.saldo;
    }

    public abstract void atualiza();
}
```

Classes concretas implementando o método abstrato:

```java
class ContaCorrente extends Conta {
    public void atualiza() {
        this.retira(50); // exemplo fictício
    }
}

class ContaPoupanca extends Conta {
    public void atualiza() {
        this.deposita(this.saldo * 0.01); // exemplo fictício
    }
}
```

Com isso, você garante que qualquer objeto referenciado pela classe abstrata tenha o método `atualiza`, permitindo o uso seguro do **polimorfismo**:

```java
Conta[] contas = new Conta[2];
contas[0] = new ContaCorrente();
contas[1] = new ContaPoupanca();

for (Conta c : contas) {
    c.atualiza(); // chama o método certo para cada conta
}
```

---

Dessa forma, as **classes abstratas** e **métodos abstratos** ajudam a manter o código organizado, consistente e seguro, garantindo que regras específicas sejam sempre implementadas pelas subclasses concretas.