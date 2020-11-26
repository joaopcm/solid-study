# SOLID

- Single Responsabiliity Principle
- Open Closed Principle
- Liskov Substitution Principle
- Interface Segregation Principle
- Dependency Inversion Principle

## O que fazem?

- Separam responsabilidades
- Diminuem acoplamentos
- Facilitam na refatoração
- Estimulam o reaproveitamento do código

## Referências

- https://medium.com/backticks-tildes/the-s-o-l-i-d-principles-in-pictures-b34ce2f1e898

---

## Single Responsability Principle

- Uma classe deve ter uma única responsabilidade para executar
- God Class são classes que fazem de tudo. Podem parecer sofisticado, mas no momento
  em que as resposabilidades começam a se misturar, alterar essa classe fica muito mais
  difícil e toda alteração acaba sendo introduzida com um certo nível de incerteza (ainda
  mais sem testes automatizados)
- esse princípio não é somente aplicável à classes, mas sim à qualquer instância
  que executa um trecho de código
- Coloque no nome da função ou do componente tudo que ele está fazendo. Se tiver dificuldades porque ficou algo bizarro, talvez essa entidade tenha responsabilidades demais. Se você estiver no começo do processo, erre bastante, pois esse é o melhor caminho para aprender como dividir responsabilidades. Só se lembre dessa parte para evitar passar por isso novamente porque dá mais trabalho refatorar que separar responsabilidades.

### Exemplo

```ts
class Order {
  // Handles order information
  public calculateTotalSum() {
    /** ... */
  }
  public getItems() {
    /** ... */
  }
  public getItemCount() {
    /** ... */
  }
  public addItem() {
    /** ... */
  }
  public deleteItem() {
    /** ... */
  }

  // Show the order
  public printOrder() {
    /** ... */
  }
  public showOrder() {
    /** ... */
  }

  // Handles data
  public load() {
    /** ... */
  }
  public save() {
    /** ... */
  }
  public update() {
    /** ... */
  }
  public delete() {
    /** ... */
  }
}

// A class should have only one responsibility

class Order {
  // Handles order information
  public calculateTotalSum() {
    /** ... */
  }
  public getItems() {
    /** ... */
  }
  public getItemCount() {
    /** ... */
  }
  public addItem(item) {
    /** ... */
  }
  public deleteItem(item) {
    /** ... */
  }
}

class OrderRepository {
  // Show the order
  public printOrder(order) {
    /** ... */
  }
  public showOrder(order) {
    /** ... */
  }
}

class OrderViewer {
  // Handles data
  public load(order_id) {
    /** ... */
  }
  public save(order) {
    /** ... */
  }
  public update(order) {
    /** ... */
  }
  public delete(order) {
    /** ... */
  }
}
```

## Open Closed Principle

- Objetos ou entidades devem estar abertos para extensão, mas fechados para modificação
- Quando novos comportamentos ou funcionalidades devem ser implementadas à um
  software, devemos extender e não modificar
- Ao alterar uma classe em pleno funcionamento, corremos o risco de implementar
  bugs em algo que já funcionava

### Exemplo

```ts
class ContratoClt {
  public salario() {
    /** ... */
  }
}

class Estagio {
  public bolsaAuxilio() {
    /** ... */
  }
}

class FolhaDePagamento {
  #saldo;

  constructor() {
    this.#saldo = 0;
  }

  public calcular(contrato) {
    if (contrato instanceof ContratoClt) {
      this.#saldo = contrato.salario();
    } else if (contrato instanceof Estagio) {
      this.#saldo = contrato.bolsaAuxilio();
    }
  }
}

// What if the company wants to contemplate a legal entity contract?
interface Remuneravel {
  remuneracao: Function;
}

class ContratoClt implements Remuneravel {
  public remuneracao() {
    /** ... */
  }
}

class ContratoPj implements Remuneravel {
  public remuneracao() {
    /** ... */
  }
}

class Estagio implements Remuneravel {
  public remuneracao() {
    /** ... */
  }
}

class FolhaDePagamento {
  #saldo: number;

  constructor() {
    this.#saldo = 0;
  }

  public calcular(contrato: Remuneravel) {
    this.#saldo = contrato.remuneracao();
  }
}
```

## Liskov Substitution Principle

- Uma classe derivada deve ser substituído pela sua classe base
- Se S é um subtipo de T, então os objetos do tipo T em um programa podem ser
  substituidos pelos objetos do tipo S sem que seja necessário alterar as propriedades
  desse programa
- Evitar sobrescrever ou implementar um método que não faz nada
- Evitar lançar uma exceção inesperada
- Evitar retornar valores dos tipos diferentes da classe base
- Permite usarmos o polimorfismo com mais confiança

### Exemplo

```ts
class A {
  public getName(): string {
    return 'My name is João';
  }
}

class B extends A {
  public getName(): string {
    return 'My name is Luciano';
  }
}

const objectA = new A();
const objectB = new B();

function printName(object: A) {
  return object.getName();
}

printName(objectA); // My name is João
printName(objectB); // My name is Luciano
```

## Interface Segregation Principle

- Uma classe não deve ser forçada a implementar interfaces e métodos que não serão utilizados
- É melhor criar interfaces mais específicas que criar uma única interface genérica;

### Exemplo

```ts
interface Bird {
  public setLocalization: Function;
  public setAltitude: Function;
  public render: Function;
}

class Parrot implements Bird {
  public setLocalization(longitude, latitude) {
    /** ... */
  }

  public setAltitude(altitude) {
    /** ... */
  }

  public render() {
    /** ... */
  }
}

class Penguin implements Bird {
  public setLocalization(longitude, latitude) {
    /** ... */
  }

  public setAltitude(altitude) {
    // Does nothing... Penguins don't fly
  }

  public render() {
    /** ... */
  }
}

// After -->
interface Bird {
  public setLocalization: Function;
  public render: Function;
}

interface FlyingBird extends Bird {
  public setAltitude: Function;
}

class Parrot implements FlyingBird {
  public setLocalization(longitude, latitude) {
    /** ... */
  }

  public setAltitude(altitude) {
    /** ... */
  }

  public render() {
    /** ... */
  }
}

class Penguin implements Bird {
  public setLocalization(longitude, latitude) {
    /** ... */
  }

  public render() {
    /** ... */
  }
}
```

## Dependency Inversion Principle

- Devemos depender de abstrações, não de implementações
- Módulos de alto nível não devem depender de módulos de baixo nível. Ambos devem depender da abstração
- Abstrações não devem depender de detalhes. Detalhes devem depender de abstrações
- Inversão de dependência <> Injeção de dependência

### Exemplo

```ts
import MySQLConnection from '../database';

class PasswordReminder {
  #dbConnection;

  constructor() {
    this.#dbConnection = new MySQLConnection();
  }

  // do something
}

// with dependency injection -->
// 1. the creation of the MySQLConnection object is no longer the responsibility of the PasswordReminder class
// 2. The database connection class became a dependency that must be injected via the constructor method
// 3. It still violates the principle of dependency inversion and open-closed. What if we need to change the type of bank? That way it only works with MySQL

import MySQLConnection from '../database';

class PasswordReminder {
  #dbConnection;

  constructor(databaseConnection: MySQLConnection) {
    this.#dbConnection = databaseConnection;
  }

  // do something
}

// with dependency inversion -->
// 1. The PasswordReminder class has no idea which database the application is going to use
// 2. Classes are unclassified and the code is reusable

interface DatabaseConnectionInterface {
  public connect: Function;
}

class MySQLConnection implements DatabaseConnectionInterface {
  public connect() {
    /** ... */
  }
}

class OracleConnection implements DatabaseConnectionInterface {
  public connect() {
    /** ... */
  }
}

class SQLServerConnection implements DatabaseConnectionInterface {
  public connect() {
    /** ... */
  }
}

class PasswordReminder {
  #dbConnection;

  constructor(databaseConnection: DatabaseConnectionInterface) {
    this.#dbConnection = databaseConnection;
  }

  // do something
}
```
