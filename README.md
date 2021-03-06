# π» TypeScript-ToDoList
νμμ€ν¬λ¦½νΈλ‘ λ§λ€μ΄λ³΄λ ToDoList π

<br />

## π νλ‘μ νΈ μμ μ  μν κ³Όμ 
### π 1. νμμ€ν¬λ¦½νΈ μ€μΉ
```
  node install -g typescript
  npm install -g ts-node
```

<br />

### π 2. tsconfig.json μμ± λ° μ€μ 
```
  //tsconfig.json μμ±
  tsc --init
```

<br />

- tsconfig.json μ€μ 
```json
{
  "compilerOptions": {
    /* Basic Options */
    "target": "es5",  
    "module": "commonjs",
    "outDir": "./build",  //λΉλ λ μ΄ν μλ°μ€ν¬λ¦½νΈ νμΌμ΄ μ μ₯ λ  μ₯μ
    "rootDir": "./src",   //νμμ€ν¬λ¦½νΈ μμ€μ½λκ° μλ μ μ₯μ

    /* Strict Type-Checking Options */
    "strict": true,  

    /* Module Resolution Options */
    "esModuleInterop": true, 
    "skipLibCheck": true,  
    "forceConsistentCasingInFileNames": true 
  }
}

```

<br />

### π 3. package.json μμ± λ° μ€μ 
```
  //package.json μμ±
  npm init -y
```

<br />

- package.json μ€μ 
```json
  {
    ...
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
      "build": "tsc -w",
      "start:run": "nodemon build/index.js",
      "start": "concurrently npm:start:*"
    },
    ...
  }
```

### π nodemon, concurrently μ€μΉ
```
  npm install nodemon concurrently
```
- concurrentlyλ λμμ μ¬λ¬ λͺλ Ήμ΄λ₯Ό μ€ννκΈ° μν΄ μ¬μ©λλ€.
- nodemon: μ€μκ°μΌλ‘ μμ μ¬ν­μ λ°μν΄μ€λ€.

<br />

## π¨βπ» TodoItem: JavaScript vs TypeScript
### π JavaScript
```js
  class TodoItem {
    constructor(id, task, complete) {
      this.id = id;
      this.task = task;
      this.complete = complete;
    }

    printDetails() {
      console.log(
        `${this.id} \t ${this.task} \t ${this.complete ? "(complete)" : ""}`
      );
    }
  }
```

<br />

### π TypeScript
- νμμ€ν¬λ¦½νΈμμλ νμμ μ§μ ν΄μ£Όλκ±° μ΄μΈμ `μ κ·Ό μ§μ μ(private, public, protected)` λ±μ μ§μ ν  μ μλ€.
- μμ±μ(contructor)μμμ `μ κ·Ό μ§μ μ`λ₯Ό μ§μ νλ©΄ λ°λ‘ νλ‘νΌν° μ μν  νμ μμ΄ νλ‘νΌν°λ‘ μ μλλ€.
- ν¨μμ λ¦¬ν΄ κ°μ΄ μμΌλ©΄ λ°ν κ° νμμ `void`λ‘ μ§μ ν΄μΌ νλ€.
```ts
  class TodoItem {
    constructor(public id: number, public task: string, public complete: boolean) {
      this.id = id;
      this.task = task;
      this.complete = complete;
    }

    printDetails(): void {
      console.log(
        `${this.id} \t ${this.task} \t ${this.complete ? "(complete)" : ""}`
      );
    }
  }

  export default TodoItem;
```

<br />

## π¨βπ» TodoCollection
### π getTodoById λ©μλ
- ν¨μ λ°ν κ°μ λν νμμ `TodoItem | undefined`μ΄λ° μμΌλ‘ μ§μ νλ©΄ λ°ν κ°μ νμμ΄ TodoItem λλ undefinedλΌλ μλ―Έ
```ts
  import TodoItem from "./TodoItem";

  class TodoCollection {
    ...
    getTodoById(id: number): TodoItem | undefined {
      return this.todoItems.find((item) => item.id === id);
    }
    ...
  }

  export default TodoCollection;
```

<br />

### πββοΈ Type Annotations - Inference Around Functions
- ν¨μμ νλΌλ―Έν°λ₯Ό μ μν  λ κ° νλΌλ―Έν°μ νμμ μ§μ νμ§ μμΌλ©΄ `any` νμμ νλΌλ―Έν°κ° μ§μ  λλ€.
- ν¨μμ νλΌλ―Έν°μ νμμ μ§μ νμ§ μμΌλ©΄ μΌλ° λ³μμ λ§μ°¬κ°μ§λ‘ μλ¬΅μ μΈ any νμμ μ μ©μΌλ‘ κ²½κ³  μ¬ν­μ΄λ€.
- ν¨μμ λ°ν κ°μ λν νμμ return μ€νλ¬Έμ λ°λΌ `νμ μΆλ‘ (Type Inference)`μ΄ μ μ© λλ€.
- λ°ν κ°μ κ²½μ° retrun κ΅¬λ¬ΈμΌλ‘ `λͺμμ μΈ νμμ μ μΆ`κ° κ°λ₯νλ€.
- ν¨μμ λ°ν κ°μ΄ μμ κ²½μ° `void` νμμ λ°νμ μ μνλ€.
- ν¨μμ λ°ν κ°μΌλ‘ μ μ κ°λ₯ν `never` νμμ μ λ λ°μνμ§ μλ κ°μ νμμ λνλΈλ€. μ½κ² λ§νλ©΄ λ°νμ μνλ€λ μλ―Έ
- `void` νμμ λ³μλ‘ μ¬μ© λ  κ²½μ° `undefined`, `null` κ°λ§ λμ(assign)μ΄ κ°λ₯νλ€.
- `never`νμμ μ΄λ€ λ³μμ λ³μμλ λμ λ  μ μμ§λ§, never νμμλ μ΄λ€ νμμ κ°λ λμλ  μ μλ€.

<br />

### πββοΈ Type Annotations - Tuple
- ννμ μ΄μ©νλ©΄ λ°°μ΄μ μμ μμ κ° μμμ λν νμμ μ§μ ν  μ μλ€.
- ννμ μ ν΄μ§ κΈΈμμ λ§μ§ μμΌλ©΄ μλ¬κ° λ°μ. νμ§λ§ `push()`λ ννμ κ·μΉμ λ¬΄μν¨.
- μλ‘ λ€λ₯Έ νμμ μμλ₯Ό κ°λ λ°°μ΄μ μμμ μκ΄μμ΄ λ°μ΄ν°λ₯Ό λ£μ μ μμ§λ§, λ°λ©΄ ννμ μ ν΄μ§ μμμ λ§κ² λ£μ΄μΌ ν¨
- νν νμμ λ°°μ΄λ³΄λ€ μ μ₯λλ μμμ μμμ μμ μ μ½μ λκ³ μ ν  λ μ¬μ©
```ts
  const tuples: [string, number] = ['Jeon', 27]; //νν
  const arr: (string | number)[] = ['Jeon', 27, 'MinJae', 26]; //λ°°μ΄

  tuples[0] = 'Park' // OK
  tuples[0] = 50 // Error: Type '50' is not assignable to type 'string'.ts(2322)

  tuples[1] = 50 // OK
  
  tuples.push(100);
  console.log(tuples); // ['Park', 50, 100] 
```

<br />

### πββοΈ Generics
- μ¬μ¬μ© κ°λ₯ν ν΄λμ€, ν¨μλ₯Ό λ§λ€κΈ° μν΄ λ€μν νμμμ μ¬μ© κ°λ₯ νλλ‘ νλ κ²μ΄ `μ λ€λ¦­`μ΄λ€.
- μ λ€λ¦­μ μ΄μ©νλ©΄ λͺ¨λ  νμμ κ°μ²΄λ₯Ό λ€λ£¨λ©΄μ κ°μ²΄ νμμ λ¬΄κ²°μ±μ μ μ§ν  μ μλ€.
- μ λ€λ¦­μ ν΅ν΄ ν΄λμ€λ ν¨μ λ΄λΆμμ μ¬μ©λλ νΉμ  λ°μ΄ν°μ νμμ μΈλΆμμ μ§μ νλ€.
- μ λ€λ¦­μ΄ μ μ©λ λμ(ν΄λμ€, ν¨μ, μΈν°νμ΄μ€)μ μ μΈ μμ μ΄ μλ μμ± μμ μ μ¬μ©νλ νμμ κ²°μ νλ€.

```ts
class Box<T> {
  constructor(private fruit: T) {}
  getFruit(): T {
    return this.fruit;
  }
}
```
- μ μ½λμμ `<T>`μ μ μ΄ μ λ€λ¦­μ μ μ© μ΄λ₯Ό νμ νλΌλ―Έν°λΌκ³  νλ€.
- Tλ§κ³  μ΄λ ν μνλ²³μ μ¬μ©ν΄λ μκ΄μ μμ§λ§ κ΄μ©μ μΌλ‘ Tλ₯Ό μ¬μ©

<br />

```ts
const box: Box<Orange> = new Box(new Orange(5));
```
- μμ μ½λλ₯Ό λ³΄λ©΄ boxλ₯Ό μ μνλ©΄μ boxμ νμμΌλ‘ Boxμ νμμ μ λ€λ¦­μΌλ‘ `<Oragne>`λ₯Ό μ§μ ν¨ μ€μ  μΈμ€ν΄μ€ν νλ©΄μ Orange κ°μ²΄λ₯Ό μμ±ν΄μ λ£μ

<br />

```ts
class Box<Orange> {
  constructor(private fruit: Orange) {}
  getFruit(): Orange {
    return this.fruit;
  }
}
```
- κ²°κ³Όμ μΌλ‘ TλΌκ³  μ§μ λ νμλ€μ λͺ¨λ Orange νμμ΄ λλ€.

<br />

### πββοΈ type alias
- μλ‘μ΄ νμμ μ μνλ λ°©λ²μ type aliasμ interfaceλ₯Ό μ μνλ λ κ°μ§ λ°©μμ΄ μλ€.
- type aliasλ₯Ό μ΄μ©νλ©΄ κ°μ²΄, κ³΅μ©μ²΄(Union), νν(Tuple), κΈ°λ³Έ νμμ νμμ λ³μΉ­μ μμ±ν  μ μλ€.
- type aliasλ μ λ€λ¦­μ μ¬μ©μ΄ κ°λ₯νλ©°, μ€μ€λ‘ μ°Έμ‘°νλ κ²λ κ°λ₯νλ€.
```ts
type MyNumber = number;
const n: MyNumber = 10;

type Container<T> = { value: T };

type User = { name: string, age: number };
const testUser: User = { name:"Kim", age:20 };
//=== const testUser: { name: string, age: number } = { ... }
```

<br />

## π¨βπ» TodoConsole
### πββοΈ inquirer λΌμ΄λΈλ¬λ¦¬ μ€μΉ
- inquirer: Interactive user promptλ₯Ό κ΅¬νμ μν λΌμ΄λΈλ¬λ¦¬λ‘ λ£¨λΉ, νμ΄μ¬, μλ°μ€ν¬λ¦½νΈ λ± μ¬λ¬κ°μ§ μΈμ΄λ₯Ό μ§μνκ³  μΌλ°μ μΈ μ¬μ©μ μλ ₯ κ΅¬ν, μ²΄ν¬λ°μ€, λΌλμ€ λ²νΌ λ± κ΅¬νμ΄ νΈλ¦¬νλ€.
- λ¨, TypeScriptμμλ @types/inquirerμ μΆκ°μ μΌλ‘ μ€μΉν΄μΌ νλ€.
```
  npm i inquirer @types/inquirer
```

<br />

### πββοΈ inquirer μ¬μ© μμ 
- src/model/TodoConsole.ts
```ts
  import * as inquirer from 'inquirer';

  class TodoConsole {
    ...
    promptUser(): void {
      console.clear();
      
      this.displayTodoList();

      inquirer.prompt({
        type: 'list',
        name: 'command',
        message: 'Choose option',
        choices: Object.values(Commands),
      }).then((answers) => {
        if(answers['command'] !== Commands.Quit) {
          this.promptUser();
        }
      });
    }
  }

  export default TodoConsole;
```

<br />

- src/model/Command.ts
```ts
  export enum Commands {
    Quit = 'Quit',
    Add = 'Add',
  }
```

<br />

- μ£Όμν  μ μ nodemon, concurrentlyμ μ¬μ©ν΄μ npm startλ₯Ό νλ©΄ μ½κ°μ μ€λ₯κ° μμ
- node build/index.js λ‘ λΉλ νκ³  μ€νν΄μΌ λλ€.

<br />

### πββοΈ enum
- μ΄κ±°ν(enum) νμμ `μμ`λ€μ κ΄λ¦¬νκΈ° μν κ°μ²΄λ‘ μμμ μ§ν©μ μ μνλ€.
- μΌλ° κ°μ²΄λ μμ±μ λ³κ²½μ νμ©νμ§λ§ μ΄κ±°νμ μμ±μ λ³κ²½μ νμ©νμ§ μλλ€.
- μ΄κ±°νμ μμ±μ κΈ°λ³Έμ μΌλ‘ `μ«μ`, `λ¬Έμμ΄`λ§ νμ©νλ€.
- μ΄κ±°νμ μ΄μ©νλ©΄ μμμ μλ₯Ό μ νν  μ μμΌλ©° μ½λμ κ°λμ±μ λμΌ μ μλ€.
```ts
  const korean = 'ko';
  const english = 'en';
  const japanese = 'ja';

  type LanguageCode = 'ko' | 'en' | 'ja';

  const code: LanguageCode = korean;
```
- μ μ½λλ₯Ό μμ νλ©΄
```ts
enum LanguageCode {
  korean = 'ko',
  english = 'en',
  japanese = 'ja',
}

const code: LanguageCode = LanguageCode.korean;
```

<br />

## π¨βπ» Union Type/Type Guard
### πββοΈ Union Type
- TypeScriptλ νμλ€μ μ‘°ν©μ ν΅ν΄ μλ‘μ΄ νμμ μ μν  μ μμΌλ©° Union Typeλ κ·Έμ€ νλμ΄λ€.
- Union Typeμ νμ μ μΈμ νλ μ΄μμ νμμ μ§μ νκ³  ν΄λΉ νμ μ€μ νλμΌ μ μμμ λνλΈλ€.
- Union Typeμ μ μλ `|` μ°μ°μλ₯Ό μ΄μ©ν΄ μ μνλ€.
- Union Typeμ λ©€λ² μ¬μ©μ μ μλ λͺ¨λ  νμμ κ³΅ν΅μ μΈ λ©€λ²λ€λ§ μ¬μ©ν  μ μλ€.

<br />

### πββοΈ Type Guard
- Type Guardλ νΉμ  μμ­(λΈλ‘) μμμ ν΄λΉ λ³μμ νμμ νμ μμΌμ£Όλ κΈ°λ₯μ΄λ€.
- Union Typeμ μ μλ κ° νμμ΄ κ°λ κ³ μ  λ©€λ²λ μ¬μ©ν  μ μλ€.
- νΉμ  μμ­μμ κ° νμμ΄ κ°λ κ³ μ  λ©€λ²μ λν μ¬μ©μ Type Guardλ₯Ό μ΄μ©ν©λλ€.
- Type Guardλ μ¬μ©μκ° μ μ νκ±°λ `number`, `string`, `boolean`, `Symbol`μ κ²½μ° `typeof` μ°μ°μλ₯Ό μ΄μ©νλ€.
```ts
  let collection: number[] | string;
  collection = 'TypeScript';

  collection.split(''); //μ¬μ©ν  μ μμ. μ? μ«μλ°°μ΄μ splitμ΄λΌλ λ©μλλ₯Ό κ°κ³ μκΈ° μκΈ° λλ¬Έ

  //Type Guard
  if (typeof collection === 'string') {
    //collectionμ΄ stringμ΄λΌλκ² trueλΌλ©΄ μ΄ λΈλ­ μμμλ split λ©μλλ₯Ό μ¬μ©ν  μ μλ€.
    collection.split('');
  }
```

<br />