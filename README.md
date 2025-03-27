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