<h1 align="center">S.O.L.I.D</h1>

**S.O.L.I.D** são um conjunto de cinco princípios para ajudar a escrever código de forma mais organizada.

Os 5 principios são:

- [Single Responsibility](#single-responsibility)
- [Open/Closed](#openclosed)
- [Liskov Substitution](#liskov-substitution)
- [Interface Segregation](#interface-segregation)
- [Dependency Inversion](#dependency-inversion)

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

A class deve estar aberta para extensão, mas fechada para modificação. Ou seja, você deve poder adicionar novas funcionalidades sem alterar o código existente, para evitar quebrar o que já está funcionando.

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
Qualquer classe filha pode ser usada no lugar da classe pai sem causar erros no código. Ou seja, a class pai deve ser o mais genérica possível.

Imagine o seguinte cenário:

Vamos criar uma Class pai (superclass) **Animal** e algumas classes filhas:

<br>

### Sem utilizar o principle

```java
//Superclass
public class Animal {
    
    public void fly(){
        System.out.println("The animal is flying");
    }
}

//classes filhas
public class Eagle extends Animal {}

public class Picapau extends Animal {}

public class Crow extends Animal{}

public class Lion extends Animal {}
```

<br>

```java
public class App {
    public static void main(String[] args){
        
        Animal animal2 = new Eagle();
        Animal animal3 = new Crow();
        Animal animal4 = new Picapau();
        Animal animal1 = new Lion();

        animal1.fly();
        animal2.fly();
        animal3.fly();
        animal4.fly(); // Lion is not supposed to fly - ERROR!!
    }
}
```

⚠️ Segundo o liskov substitution principle, a superclass pode ser substituída por qualquer classe filha. Nesse caso, um "leao" NÃO pode voar. Portanto, violamos o principle!

Percebe que nem todo animal pode voar? Entao não faz sentido colocar esse method na classe pai. Faz mais sentido colocar esse method apenas nas classes filhas/animais, que podem voar.

<br>

### Utilizando o principle

```java
//superclass
public class Animal {
    
    public void move(){
        System.out.println("The animal is moving");
    }
}


//classes filhas
public class Eagle extends Animal {

    public void fly(){
        System.out.println("eagle is flying");
    }
}


public class Picapau extends Animal {

    public void fly(){
        System.out.println("Picapau is flying");
    }
}

public class Crow extends Animal{

    public void fly(){
        System.out.println("Crow is flying");
    }
}

public class Lion extends Animal {}
```

<br>

```java
public class App {
    public static void main(String[] args){
        
        Animal animal2 = new Eagle();
        Animal animal3 = new Crow();
        Animal animal4 = new Picapau();
        Animal animal1 = new Lion();

        //all animals can move
        animal1.move();
        animal2.move();
        animal3.move();
        animal4.move();
    }
}
```

📖 A classe pai contém apenas **métodos que todos os animais podem usar**. Métodos específicos, como fly(), foram movidos para as classes que podem realmente executar essa ação. Isso respeita o princípio e evita erros no código.


<hr>
<br>

## Interface Segregation
**Uma classe não deve ser obrigada a usar métodos de uma interface que não precisa.** É melhor ter várias interfaces menores e específicas, permitindo que as classes implementem apenas o que realmente precisam. Isso deixa o código mais organizado e fácil de entender.

Exemplo:

### Sem utilizar o principle

Temos uma interface "grande" com todos os methods relacionados a "phone"

```java
public interface PhoneFunctionalities{
    
    public void call();

    public void whatsaAppMessage(String message);
}
```

<br>

```java
public class SmartPhone implements PhoneFunctionalities{
    @Override
    public void call() {
        System.out.println("SmartPhone is calling to someone");
    }

    @Override
    public void whatsaAppMessage(String message) {
        System.out.println(message);
    }
}


public class OldPhone implements PhoneFunctionalities{

    @Override
    public void call() {
        System.out.println("old phone is calling to someone");
    }

    //mesmo nao precisando desse method, somos OBRIGADOS a implementá-lo
    @Override
    public void whatsaAppMessage(String message) {
        throw new RuntimeException("Old phone does not have WhatsApp");
    }
}
```

⚠️ Perceba que os "telefones antigos" não possuem serviços avançados. Eles apenas fazem e recebem ligações. Não faz sentido voce ser obrigado a implementar um method que voce nao vai utilizar naquela class.

<br>

### Utilizando o principle

Aqui, vamos criar interfaces menores para atender as classes de forma específica

```java
//interface1
public interface OldPhoneFunctionalities{
    
    public void call();
}

//interface2
public interface SmartPhoneFunctionalities{
    
    public void call();
    public void whatsaAppMessage(String message);
}
```

<br>


```java
//class1
public class SmartPhone implements SmartPhoneFunctionalities{
    @Override
    public void call() {
        System.out.println("SmartPhone is calling to someone");
    }

    @Override
    public void whatsaAppMessage(String message) {
        System.out.println(message);
    }
}


//class2
public class OldPhone implements OldPhoneFunctionalities{

    @Override
    public void call() {
        System.out.println("old phone is calling to someone");
    }
}
```

📖 Agora sim, não precisamos implementar methods que nao vamos usar.

<hr>
<br>

## Dependency Inversion
O Princípio de Inversão de Dependência afirma que as classes de alto nível não devem depender de classes de baixo nível, **mas sim de abstrações, como interfaces**. Isso ajuda a tornar o código mais flexível e fácil de manter.

<br>

Exemplo:

Imagine um programa que vai enviar mensagens para o cliente. A princípio temos as seguintes formas de enviar mensagens:

`email`, `SMS`

<br>

### Sem utilizar o principle

Aqui, definimos a mensagem padrao de cada tipo de notificacao.

```java
//class 1
public class EmailNotifications {
    
    public String sendEmailMessage(){
        return "EMAIL message insana!";
    }
}

//class 2
public class SmsNotifications {
    
    public String sendSmsMessage(){
        return "SMS message insana!";
    }
}
```

<br>

Aqui, vamos ter a class responsavel em de fato enviar a mensagem ao cliente.


```java
public class SendMessages {
    
    //attributes
    private EmailNotifications emailNotifications; //essa class depende dessa Class "EmailNotifications"
    private SmsNotifications smsNotifications; //essa class depende dessa Class "SmsNotifications"

    //constructors
    public SendMessages(){
        this.emailNotifications = new EmailNotifications();
        this.smsNotifications = new SmsNotifications();
    }

    //methods
    public void enviarMensagemCompleta(String messageType){
        System.out.println("Nova mensagem enviada!");

        if(messageType.equals("SMS")){
            System.out.println("Message: " + this.smsNotifications.sendSmsMessage());
        }
        if(messageType.equals("EMAIL")){
            System.out.println("Message: " + this.emailNotifications.sendEmailMessage());
        }
    }
}
```

⚠️ Perceba que essa class está DEPENDENDO das classes "EmailNotifications" e "SmsNotifications". Isso fere o principle.

Se você quisesse criar outra forma de enviar uma notificacao ao cliente, exemplo por ligação, voce teria que adicionar mais um attribute nessa Class. E ela dependeria de mais 1 Class. Se ligou??

<br>

```java
public class SendMessages {
    
    //attributes
    private EmailNotifications emailNotifications; //essa class depende dessa Class "EmailNotifications"
    private SmsNotifications smsNotifications; //essa class depende dessa Class "SmsNotifications"
    private PhoneCall phoneCall; //essa class depende dessa Class "phoneCall"

    //constructors
    public SendMessages(){
        this.emailNotifications = new EmailNotifications();
        this.smsNotifications = new SmsNotifications();
        this.phoneCall = new PhoneCall();
    }

    //methods
    public void enviarMensagemCompleta(String messageType){
        System.out.println("Nova mensagem enviada!");

        if(messageType.equals("SMS")){
            System.out.println("Message: " + this.smsNotifications.sendSmsMessage());
        }
        if(messageType.equals("EMAIL")){
            System.out.println("Message: " + this.emailNotifications.sendEmailMessage());
        }
        if(messageType.equals("PHONE")){
            System.out.println("Message: " + this.phoneCall.call());
        }
    }
}
```

Para evitar essa dependencia de attributes, vamos utilizar o **dependency inversion principle**.

<br>

### Utilizando o principle

1. Criamos uma interface. Assim, **a class responsavel por enviar as messages vai ser dependente apenas da interface** e não de TODAS aquelas classes.


    ```java
    public interface Notifications {
        
        public String defaultMessage();
    }
    ```

<br>

2. Implementamos essa interface nas nossas classes de notificacao

    ```java
    public class EmailNotifications implements Notifications{

        @Override
        public String defaultMessage() {
            return "Email notification insane!";
        }
    }


    public class SmsNotifications implements Notifications{

        @Override
        public String defaultMessage() {
            return "SMS notification insane!";
        }
    }


    public class PhoneCall implements Notifications{

        @Override
        public String defaultMessage() {
            return "PHONE notification insane!";
        }
    }
    ```

<br>

3. Agora sim, nossa classe vai depender apenas da interface

    ```java
    public class SendMessages {
        
        //attributes
        private Notifications notification; //dependemos apenas dessa interface

        //constructors
        public SendMessages(Notifications notification){
            this.notification = notification;
        }

        //methods
        public void enviarMensagemCompleta(){
            System.out.println(this.notification.defaultMessage());
        }
    }
    ```

    📖 Olha a diferença! Aqui, dependemos apenas da interface. Nós **invertemos o tipo de dependencia**. Agora, dependemos apenas de uma interface.

<br>

4. Se quiséssemos enviar as mensagens, ficaria assim:

    ```java
    public class App {
        public static void main(String[] args){

            EmailNotifications email = new EmailNotifications();
            SmsNotifications sms = new SmsNotifications();
            PhoneCall phone = new PhoneCall();

            SendMessages sendEmailMessage = new SendMessages(email);
            sendEmailMessage.enviarMensagemCompleta();

            SendMessages sendSmsMessage = new SendMessages(sms);
            sendSmsMessage.enviarMensagemCompleta();

            SendMessages sendPhoneMessage = new SendMessages(phone);
            sendPhoneMessage.enviarMensagemCompleta();
        }
    }
    ```