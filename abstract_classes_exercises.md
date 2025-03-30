# Exercícios – Classes Abstratas

**1.** Observe o comportamento da classe `Conta` em seu sistema. Considere se faz sentido instanciar diretamente um objeto do tipo `Conta`, dado que, na prática, só trabalhamos com `ContaCorrente`, `ContaPoupanca`, entre outros tipos específicos. Analise também quais métodos da classe `Conta` são genéricos demais e poderiam ser definidos como **métodos abstratos**. Faça as alterações necessárias, tornando `Conta` uma **classe abstrata** e marque os métodos adequados com a palavra-chave `abstract`. Em seguida, no `main`, tente instanciar `Conta` e veja o que acontece na **compilação**.

---

**2.** Mesmo após tornar `Conta` abstrata, ainda faz sentido ter métodos que **recebam referências do tipo `Conta`** como parâmetro? Reflita sobre o papel do **polimorfismo** nesse cenário e avalie se a linguagem Java permite esse tipo de uso, mesmo quando não é possível instanciar diretamente a classe abstrata.

---

**3.** Agora, vá até a classe `ContaPoupanca` e **remova o método `atualiza`** dela. Isso fará com que ela herde a versão definida em `Conta`. Em seguida, torne o método `atualiza(double taxaSelic)` da classe `Conta` um **método abstrato**. Tente compilar o sistema novamente. O que acontece com a classe `ContaPoupanca`? Por que esse erro aparece? O que isso nos ensina sobre métodos abstratos?

```java
abstract class Conta {
    // outros atributos e métodos
    abstract void atualiza(double taxaSelic);
}
```

---

**4.** Reescreva o método `atualiza` dentro da classe `ContaPoupanca`, garantindo que ela agora **implemente corretamente** o método abstrato herdado de `Conta`. Após isso, compile novamente e verifique se o erro desaparece.

---

**5.** *(Opcional)* Sabemos que, ao herdar um método abstrato, a subclasse é **obrigada a reescrevê-lo**. No entanto, existe alguma outra forma da classe `ContaCorrente` compilar **sem implementar** explicitamente o método `atualiza`? Pesquise ou experimente uma estrutura que torne isso possível.

---

**6.** *(Opcional)* Reflita sobre a utilidade do método `atualiza` dentro da classe `Conta`. Se ele **não tem nenhuma implementação concreta**, por que deixá-lo lá? E se simplesmente o **removêssemos da classe pai** e o deixássemos apenas nas classes filhas, o que mudaria? Esse código ainda funcionaria da mesma forma?

---

**7.** *(Opcional)* Se a classe `Conta` é abstrata, por que o código `new Conta[10]` **compila normalmente**? O que estamos realmente criando nesse caso? Isso infringe de alguma forma a regra que diz que **não se pode instanciar classes abstratas**?

---

**8.** *(Opcional)* É possível **chamar o método `atualiza` de dentro da própria classe `Conta`**, mesmo ele sendo abstrato? Tente fazer isso e reflita sobre o que o compilador aponta. Por que não é permitido? O que esse comportamento diz sobre a **função dos métodos abstratos**?

---

