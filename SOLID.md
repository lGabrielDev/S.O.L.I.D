<h1 align="center">S.O.L.I.D</h1>

Os princípios **S.O.L.I.D** são um conjunto de cinco diretrizes para ajudar a escrever código de forma mais organizada.

<hr>
<br>

## Single Responsibility

No princípio da Single Responsibility (Responsabilidade Única), **cada classe deve se concentrar em uma única tarefa**. Isso torna o código mais fácil de manter, já que cada classe faz uma coisa bem definida.

Ex:

- PrintSomethingClass --> Classe contendo apenas methods de printar.
- SaveMessageClass  --> Classe para salvar mensagens em um arquivo.

**Cada classe possui uma única terefa/responsabilidade.**

<hr>
<br>

## Open/Closed

O código deve estar aberto para extensão, mas fechado para modificação. Ou seja, você deve poder adicionar novas funcionalidades sem alterar o código existente, para evitar quebrar o que já está funcionando.

<br>

Exemplo:

Imagine que voce vai usar um method que vai printar símbolos diferentes. Dependendo do tipo do objeto, vamos printar um símbolo diferente.

Vamos trabalhar com esses símbolos:
`!` `?` `$`

<br>

Para cada um deles, vamos criar uma Class:

```java
public class ExclamationMark {}

public class QuestionMark {}

public class DollarSign {}
```

*Nao precisa criar nenhum attribute. Apenas crie as classes*

<br>

### Sem utilizar o principle:


```java
public class App{
    public static void main(String args[]){
        
        List<Object> shapes = List.of(
            new DollarSign(),
            new ExclamationMark(),
            new QuestionMark()
        );

        App.printAllShapes(shapes);
    }

         
    //method "closed" que nao deveríamos alterar
    public static void printAllShapes(List<Object> shapes){
        for(Object i : shapes){
            if(i.getClass().equals(DollarSign.class)){
                System.out.print("$ ");
            }
            if(i.getClass().equals(ExclamationMark.class)){
                System.out.print("! ");
            }
            if(i.getClass().equals(QuestionMark.class)){
                System.out.print("? ");
            }
        }
    }
}
```

Perceba que se voce tentar criar uma nova Class de um outro símbolo, vamos pegar o `%` como exemplo, voce vai precisar modificar o method `printAllShapes()`.

```java
import java.util.List;

public class App{
    public static void main(String args[]){
        
        List<Object> shapes = List.of(
            new DollarSign(),
            new ExclamationMark(),
            new QuestionMark(),
            new PercentMark()
        );
        App.printAllShapes(shapes);
    }

         
    //method "closed" que nao deveríamos alterar
    public static void printAllShapes(List<Object> shapes){
        for(Object i : shapes){
            if(i.getClass().equals(DollarSign.class)){
                System.out.print("$ ");
            }
            if(i.getClass().equals(ExclamationMark.class)){
                System.out.print("! ");
            }
            if(i.getClass().equals(QuestionMark.class)){
                System.out.print("? ");
            }
            //ALTERAMOS aqui
            if(i.getClass().equals(PercentMark.class)){
                System.out.print("% ");
            }
        }
    }
}
```

Isso nao eh a melhor maneira. Vamos aplicar o open/closed principle agora.

<br>

### Utilizando o principle:


1. Criamos uma interface para vincular todas as Class.

    ```java
    public interface MarkPrinter {
        
        public void print();
    }

    ```

<br>

2. Implementamos a interface em todas as Class.

    ```java
    public class DollarSign implements MarkPrinter{

        @Override
        public void print() {
            System.out.print("$ ");
        }
    }

    public class QuestionMark implements MarkPrinter{

        @Override
        public void print() {
        System.out.print("? ");
        }
    }

    public class ExclamationMark implements MarkPrinter{

        @Override
        public void print() {
        System.out.print("! ");
        }
    }
    ```

<br>


3. Deixamos o method `printAllShapes()` aberto para extenções, mas fechado para modificações.

    ```java
    public class App{
        public static void main(String args[]){
            
            List<MarkPrinter> shapes = List.of(
                new DollarSign(),
                new ExclamationMark(),
                new QuestionMark(),
                new PercentMark()
            );

            App.printAllShapes(shapes);
        }

            
        //method "closed" para modificacao. Ele nao deve ser alterado, pois ja esta funcionando bem
        public static void printAllShapes(List<MarkPrinter> shapes){
            for(MarkPrinter i : shapes){
                i.print();
            }
        }
    }
    ```


    📖 Agora perceba que se voce quiser adicionar outras classes para printar, o method `printAllShapes()` não precisa ser alterado. 

<br>

4. Vamos adicionar um novo símbolo:

    ```java
    public class PercentMark implements MarkPrinter{

        @Override
        public void print() {
            System.out.print("% ");
        }  
    }
    ```

<br>

```java
public class App{
    public static void main(String args[]){
        
        List<MarkPrinter> shapes = List.of(
            new DollarSign(),
            new ExclamationMark(),
            new QuestionMark(),
            new PercentMark() //adicionamos um novo simbolo aqui
        );

        App.printAllShapes(shapes);
    }

         
    //Ele está FECHADO para mdificacoes, mas está aberto para extenções.
    public static void printAllShapes(List<MarkPrinter> shapes){
        for(MarkPrinter i : shapes){
            i.print();
        }
    }
}
```


O method não foi alterado. 😎


<hr>
<br>

## Liskov Substitution

<hr>
<br>

## Interface Segregation

<hr>
<br>

## Dependency Inversion