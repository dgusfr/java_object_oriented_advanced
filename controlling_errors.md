#  Exceções: Controlando os erros


### **10.1 – Exceção**

Retomando o exemplo das **Contas** do capítulo 6: se alguém tentar **sacar** além do limite, o que ocorre? O sistema pode até mostrar uma mensagem de erro, mas quem chamou o método `saca` não fica sabendo que a operação falhou. Como “avisar” para o chamador que o método não conseguiu fazer o que deveria?

Em Java, os métodos devem comunicar suas condições, indicando quando algo não pôde ser feito. Imagine o seguinte:

```java
Conta minhaConta = new Conta();
minhaConta.deposita(100);
minhaConta.setLimite(100);
minhaConta.saca(1000);
// O saldo fica em -900, 100 ou 0? O método saca funcionou?
```

**Quem realmente sabe lidar com o erro muitas vezes é quem chamou o método**, não o próprio método `saca`. Logo, precisamos de um mecanismo para sinalizar esse problema.

No passado, uma solução era retornar `boolean`:

```java
boolean saca(double quantidade) {
    if (quantidade > this.saldo + this.limite) {
        System.out.println("Não posso sacar fora do limite!");
        return false;
    } else {
        this.saldo -= quantidade;
        return true;
    }
}
```

E, ao usar:

```java
if (!minhaConta.saca(1000)) {
    System.out.println("Não saquei");
}
```

Isso **não resolve** situações onde precisamos identificar erros mais complexos, como valores negativos, códigos de falha, etc. Em Java, empregamos as **exceções** para tratar situações que **fujam da regra normal** de execução, obrigando o chamador a lidar com o problema.

---

### **10.2 – Matemático profissional?**

Veja outro exemplo ao **dividir por zero**:

```java
public class TestandoADivisao {
    public static void main(String args[]) {
        int i = 5571;
        i = i / 0;
        System.out.println("O resultado é " + i);
    }
}
```

O que acontece ao rodar esse código? Ele lança uma **ArithmeticException**, pois a operação é indefinida. Sem tratamento, o programa encerra abruptamente.

---

### **10.3 – Abusando de uma array**

Um caso frequente é acessar indevidamente um índice de array, gerando uma **ArrayIndexOutOfBoundsException**. No exemplo:

```java
public void testaArray() {
    int nossaArray[] = new int[4];
    nossaArray[0] = 10;
    nossaArray[1] = 20;
    nossaArray[2] = 30;
    nossaArray[3] = 40;

    for(int i = 0; i != 5; i++) {
        System.out.println(nossaArray[i]);
    }
}
```

O laço `for` tenta acessar a posição `4`, que não existe, resultando em exceção. Podemos tratar com `try/catch`:

```java
public void testaArray() {
    int nossaArray[] = new int[4];
    nossaArray[0] = 10;
    nossaArray[1] = 20;
    nossaArray[2] = 30;
    nossaArray[3] = 40;

    try {
        for(int i = 0; i != 5; i++) {
            System.out.println(nossaArray[i]);
        }
    } catch (ArrayIndexOutOfBoundsException ex) {
        System.out.println("O erro " + ex.getMessage() + " ocorreu.");
    }
}
```

Observe que, nesse caso, a **causa real** é um `for` mal escrito. Como tratar essa exceção não é obrigatório, chamamos esse tipo de exceção de **unchecked**: poderíamos simplesmente corrigir o laço.

---

### **Capítulo 10 - Exceções – Controlando os erros - Página 73**  
*(indica a referência ao texto original)*

---

### **10.4 – Outro tipo de exceção: Checked Exceptions**

No exemplo acima, mesmo sem `try/catch`, o código compila. Entretanto, há exceções em Java que **obrigam** quem chama o método a tratá-las ou declará-las. Um caso comum é a manipulação de arquivos:

```java
public static void metodo() {
    new java.io.FileReader("arquivo.txt");
}
```

Isso não compila, pois `FileReader` pode gerar uma **FileNotFoundException**. Precisamos:

1. **Tratar com try/catch**:
   ```java
   public static void metodo() {
       try {
           new java.io.FileReader("arquivo.txt");
       } catch (java.io.FileNotFoundException fnfex) {
           System.out.println("Não foi possível abrir o arquivo para leitura");
       }
   }
   ```
2. **Delegar com throws**:
   ```java
   public static void metodo() throws java.io.FileNotFoundException {
       new java.io.FileReader("arquivo.txt");
   }
   ```

Quem “usa” esse método agora sabe que existe a possibilidade de erro ao abrir o arquivo. Decidir **onde** tratar a exceção depende de quem tem **capacidade** de resolver ou tomar uma decisão sobre o problema.

---

### **10.5 – Mais de um erro**

Podemos tratar várias exceções seguidas no mesmo `try`:

```java
try {
    objeto.metodoQuePodeLancarIOeSQLException();
} catch (IOException e) {
    // trata problemas de I/O
} catch (SQLException e) {
    // trata problemas de SQL
}
```

Também é possível usar `throws` para informar que o método pode lançar mais de uma exceção:

```java
public void abre(String arquivo) throws IOException, SQLException {
    // ...
}
```

Ou mesclar as abordagens (tratar algumas e repassar outras). Exceções do tipo **unchecked** não precisam ser declaradas, embora possamos fazê-lo para fins de documentação.

---

### **10.6 – E finalmente... (finally)**

Além de `try` e `catch`, podemos ter um bloco **`finally`**, que é executado **sempre**, independente de ter ocorrido exceção ou não. Por exemplo:

```java
try {
    // bloco try
} catch (IOException ex) {
    // bloco catch 1
} catch (SQLException sqlex) {
    // bloco catch 2
} finally {
    // bloco finally
    // executa sempre (útil para fechar arquivos, conexões, etc.)
}
```

---

### **10.7 – Criando novas exceções**

Podemos lançar explicitamente uma exceção com a palavra-chave **`throw`**, evitando que o chamador esqueça de checar retornos ou códigos de erro.

#### Exemplo: classe Carro

```java
public class Carro {
    private double velocidade;
    private double velocidadeMaxima = 120;
    // ...

    void acelera(double valor) {
        if (valor + this.velocidade > this.velocidadeMaxima) {
            throw new RuntimeException();
        } else {
            this.velocidade += valor;
        }
    }
}
```

Isso lança uma `RuntimeException`, que é muito genérica. Para ser mais claro, poderíamos lançar `IllegalArgumentException`:

```java
void acelera(double valor) {
    if (valor + this.velocidade > this.velocidadeMaxima) {
        throw new IllegalArgumentException("Você acelerou além do limite!");
    }
    this.velocidade += valor;
}
```

Podemos ainda criar uma **exceção específica**, por exemplo:

```java
public class VelocidadeUltrapassadaException extends RuntimeException {
    public VelocidadeUltrapassadaException(String message) {
        super(message);
    }
}
```

Então:

```java
void acelera(double valor) {
    if (valor + this.velocidade > this.velocidadeMaxima) {
        throw new VelocidadeUltrapassadaException("Ultrapassou o limite!");
    }
    this.velocidade += valor;
}
```

No `catch`, podemos capturar esse tipo de erro:

```java
Carro c = new Carro();
try {
    c.acelera(100);
    c.acelera(100);
} catch (VelocidadeUltrapassadaException ex) {
    System.out.println("Problemas: " + ex.getMessage());
}
```

Para torná-la **checked**, basta não herdar de `RuntimeException`, e sim de outra classe que force tratamento (por exemplo, `Exception`). Nesse caso, o chamador precisará usar `throws` ou `try/catch`.

Também podemos enriquecer a exceção com mais atributos (por exemplo, a velocidade máxima, o valor tentado, etc.):

```java
public class VelocidadeUltrapassadaException extends RuntimeException {
    private double valor;
    private double maximo;

    public VelocidadeUltrapassadaException(double valor, double maximo) {
        this.valor = valor;
        this.maximo = maximo;
    }

    @Override
    public String getMessage() {
        return "Você tentou ir para a velocidade " + this.valor +
               ", sendo que a máxima é " + this.maximo;
    }
}
```

Assim, o bloco de `catch` pode exibir **detalhes adicionais** sobre o erro.

---

#### **catch e throws**

Uma prática ruim é sempre capturar ou lançar `Exception`, pois isso **generaliza** o erro. Quem recebe uma `Exception` não sabe ao certo o que aconteceu. O ideal é usar **exceções específicas**, deixando o código mais claro e o tratamento mais preciso.

---

