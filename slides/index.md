- title : Typescript
- description : Typescript-intro
- author : Jonathan Hedrén
- theme : night
- transition : default

***

# Typescript

---

- "Superset" av JavaScript som kompilerar till JavaScript
- Open Source, cross platform
- Utvecklas av Microsoft (Anders Hejlsberg)

---

#### Projekt/Editors/IDEs

tsconfig.json

- Visual Studio
- Visual Studio Code
- Sublime Text
- Atom
- Eclipse
- Emacs
- WebStorm
- Vim

***

## Mål

- Erbjuda ett (valfritt) typsystem för JavaScript
- Erbjuda en möjlighet att använda kommande JavaScript-features

***

## Typescripts typsystem

- Gör större JavaScript-projekt "maintainable"
- Gör det möjligt att skapa ett mycket bra IDE-stöd
- Typescript är JavaScript; lätt att konvertera
- Typfel förhindrar inte kompilering till JavaScript

***

### Primitiva typer

- string
- number
- boolean


    [lang=ts]
    var foo: string = 'en sträng';
    var bar: number = 1;
    var baz: boolean = true;

    foo = false; // error
    bar = 'bar'; // error
    baz = 5; // error

***

### Postfix-typannotering 

    [lang=ts]
    function concat(a: string, b: string): string {
        return a + b;
    }

    var a: string = "foo";
    var b: string = "bar";

    var result: string = concat(a, b); // "foobar"

***

### Type inference

    [lang=ts]
    var foo = 1;
    foo = 'foo'; // error

***

### Arrays 
Typnamn + []

    [lang=ts]
    var foo: number[] = [];

    var bar = new Array<number>();

***

### Interfaces

Slår ihop typannoteringar i strukturer 

    [lang=ts]
    interface Foo {
        bar?: string;
        baz(): void;
    }

    var foo: Foo = {
        baz: function(): void { },
        gazonk: 2 // error
    };

    interface Bar {
        [key: string]: any;
    }

    var bar: Bar = {
        baz: 'baz',
        gazonk: 1
    };

---

Går att "inline:a"
    
    [lang=ts]
    function foo(arg: { bar: string; baz(): void; }): void {
    }

---

Funktioner
    
    [lang=ts]
    interface MyFunc {
        (arg: number): boolean;
    }

    var myFunc: MyFunc = function(arg/* type inference */) {
        return true;
    }

---

Kan "ärvas"...

    [lang=ts]
    interface Foo {
        baz: string;
    }

    interface Bar extends Foo {
        gazonk: boolean;
    }

    var bar: Bar = {
        baz: 'baz',
        gazonk: true
    };

---

...och utökas ("augmentation").

    [lang=ts]
    interface Foo {
        bar: number;
    }

    interface Foo {
        baz: string;
    }

    var foo: Foo = {
        bar: 1,
        baz: 'baz'
    };


***

### Specialtyper

---

**any**: säger till kompilatorn att inte bry sig om typen

    [lang=ts]
    var x: number = '1' as any;

---

**null**, **undefined**: samma som i JavaScript, implicit "any"

    [lang=ts]
    var x = null;
    x = 1;
    x = '2';

---

**void**: används bara för att indikera att en funktion saknar returtyp

    [lang=ts]
    function foo(): void { }

***

### Generics

    [lang=ts]
    function foo<T>(arg: T): T {
       return arg;
    }

    var bar: number = foo<number>(5);

    var baz = foo(5); // type inferred
    
    interface Foo<T, U> {
        bar(arg: T): T;
        baz: U;
    }

    var x: Foo<number, boolean> = {
        bar: function(arg) { return arg; },
        baz: false
    };

---

Typrestriktioner

    [lang=ts]
    interface Foo {
        ...
    }

    function bar<T extends Foo>(x: T): void {

    } 

***
### Union-typer

    [lang=ts]
    function foo(arg: string|number): void {
        if (typeof arg === 'string') {
            // arg = string
        }
        else {
            // arg = number
        }
    }

    interface Foo {
        bar: boolean|number;
    }

    var foo1: Foo = {
        bar: true
    };

    var foo2: Foo = {
        bar: 5
    };

***

### Tuples

    [lang=ts]
    var foo: [string, boolean] = ['bar', false];
    foo = [false, 'bar'] // error

***

### Typalias
    
    [lang=ts]
    type StringAndNumber = [string, number];
    type CallbackFn = (arg: StringAndNumber) => void;

    function foo(callback: CallbackFn): void {

    }

    type Foo<T> = { bar: T };

***

### String literals

    [lang=ts]
    type FooOrBar = 'Foo' | 'Bar';

    function handleFooOrBar(arg: FooOrBar): void {
        if (arg === 'Foo') {

        }
        else if (arg === 'Bar') {

        }
    }

    handleFooOrBar('Baz'); // error

***

### Strukturell typkompabilitet

    [lang=ts]
    interface Foo {
        value: number;
    }

    interface Bar {
        value: number; 
    }

    var x: Foo = {
        value: 1
    };

    x = { value: 2 } as Bar;

***

### Typdeklarationer (.d.ts)

Beskriver JavaScript-API:er.

DefinitelyTyped på GitHub

typings via npm

***

### declare


    [lang=ts]
    declare var $: any;

***

## "Future JavaScript"

![alt](images/ts-es.png)

***

### Klasser

    [lang=ts]
    class Foo extends Bar {
        static thing = 1;
        private baz: string = 'baz';
        constructor(a: string, private b:number, public c:boolean) {
            super(a);
            Foo.thing++;
        }
        private method1(): void {
        }
        
        method2(): void {
            super.method2();
        }
    }

---

#### get, set

    [lang=ts]
    class Foo {
        private _bar: string;

        get bar(): string {
            return this._bar;
        }

        set bar(newBar: string) {
            if (!newBar) {
                throw 'Argument required!';
            }
            this._bar = newBar;
        }
    }

    var foo = new Foo();
    var newBar = foo.bar + 'baz';
    foo.bar = newBar;

---

#### Abstrakta klasser

    [lang=ts]
    abstract class Foo {
        abstract doSomething(): void;
        protected doSomethingElse(): void {
            
        }
    }

    class Bar extends Foo {
        doSomething(): void {
            this.doSomethingElse();
        }
    }

***

### "Arrow functions"

    [lang=ts]
    var add1 = (x: number) => x + 1;
    
    var add2 = (x: number) => {
        var result = x + 2;
        return result;
    };

    var obj = () => ({ foo: 1, bar: 'bar' });

---

#### _this_

    [lang=ts]
    class Foo {
        constructor() {
            var _this = this;
            function bar() {
                console.log(this == _this); // false 
            }

            var baz = () => {
                console.log(this == _this); // true
            };
        }
    }

***

### "Params"

    [lang=ts]
    function foo(a: string, ...rest: any[]): void {

    }

***

### let

JavaScript har funktionsscope istället för blockscope:

    [lang=js]
    var foo = 123;
    if (true) {
        var foo = 456;
    }
    console.log(foo); // 456

    var foo = 123;
    function test() {
        var foo = 456;
    }
    test();
    console.log(foo); // 123

---

_let_ möjliggör blockscope:ning 

    [lang=ts]
    let foo = 123;
    if (true) {
        let foo = 456;
    }
    console.log(foo); // 123

***

### const

"immutability", sort of.
 
    [lang=ts]
    const foo = "foo";
    foo = "bar" // error

    const bar = { baz: 1 };
    bar.baz = 2;

***

### Destructuring

    [lang=ts]
    const [first, second] = ['first', 'second'];

    const obj = { a: 1, b: 'foo' };
    const {a, b} = obj;

    const [x, y, ...remaining] = [1, 2, 3, 4];
    
    const [x, , ...remaining] = [1, 2, 3, 4];

***

### for ... of

    [lang=ts]
    const stuff = ['first', 'second', 'third'];
    for (let index in stuff) {
        const item = stuff[index];
    }

    for (let item of stuff) {

    }

***

### Template-strängar

    [lang=ts]
    const value = 1;
    const formatted = `En sträng med 
                       värdet ${value}.`;
    console.log(formatted); // En sträng med värdet 1.

***

### enum

    [lang=ts]
    enum Foo {
        Gazonk,
        Bar = 1 << 1,
        Baz = 1 << 2,
        BarBaz = Bar | Baz
    }

    const x = Foo.BarBaz; // 6
    const xString = Foo[x]; // "BarBaz"

Kompilerar till JavaScript-objekt med uppslagning mellan nummer- och strängrepresentation och tvärtom.


***

### const enum 
    [lang=ts]
    const enum Foo {
        Bar,
        Baz,
        Gazonk
    }

    let x = [Foo.Bar, Foo.Baz, Foo.Gazonk];

kompilerar till

    [lang=js]
    var x = [0 /* Bar */, 1 /* Baz */, 2 /* Gazonk */];

***

### namespace

= namngivna JavaScript-objekt i det globala namespace:t

    [lang=ts]
    namespace foo.bar {

        // privat funktion
        function gazonk() {

        }

        export class Baz {
            constructor() {}
        }
    }

    const x = new foo.bar.Baz();

***

###  Moduler

Skillnaden mot namespaces: 

- deklarerar sina beroenden
- kan inte konkateneras i sammanslagen js-fil
- måste använda "module loader"; umd, amd, commonjs, es6/es2015

---

foobar.ts:

    [lang=ts]

    // privata "medlemmar"
    const baz = 'baz';
    function baz(): void { console.log(baz); }

    export function foo(): void { console.log('foo'); }
    export function bar(): void { console.log('bar'); }

---

foobarConsumer.ts:

    [lang=ts]
    import * as foobar from './foobar';

    foobar.foo(); // "foo"
    foobar.bar(); // "bar"
    foobar.baz(); // error

fooConsumer.ts:

    [lang=ts]
    import { foo } from './foobar';

    foo(); // "foo"

***

## Typescript 2.x

non-nullable types

    [lang=ts]
    const foo: number = null; // error
    const bar: number | null = null;

readonly-properties

    [lang=ts]
    class Foo {
        constructor(public readonly bar: number) {
        }
    }

***

## Resurser

- [typescriptlang.org](http://www.typescriptlang.org/)
- [gitbook.com/book/basarat/typescript](https://www.gitbook.com/book/basarat/typescript)
