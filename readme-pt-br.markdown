# ES6: Visão geral em 350 pontos chaves

[ES6 in Depth][39] é uma serie que consiste em 24 artigos, na abordagem das mudanças de sintax e features vindas do ES6. este artigo tem como objetivo resumir todo ES6, proporcionando uma visão prática em ES6, para você iniciar rapidamente em seus estudos, Eu também linkei aos artigos de ES6, alguns links para que você possa facilmente ir mais fundo em qualquer assunto que tenha interesse.

Ouvi dizer que voce gosta de pontos chave, entao eu criei um artigo contendo centenas desses bad boys. Para facilitar a busca, segue abaixo uma tabela contendo todos os assuntos abordados. Cada uma contem um ponto chave -- **obviamente**. Se voce realmente deseja que estes conceitos preencham sua mente, voce deverá se [aprofundar no assunto][39] praticando e experimentando com códigos ES6

![É hora do SHOW!][40]

# Conteudo

- [Introdução](#introducao)
- [Tooling](#tooling)
- [Assignment Destructuring](#assignment-destructuring)
- [Spread Operator e Rest Parameters](#spread-operator-e-rest-parameters)
- [Arrow Functions](#arrow-functions)
- [Templates Literais](#template-literais)
- [Objetos Literais](#objetos-literais)
- [Classes](#classes)
- [Let e Const](#let-e-const)
- [Symbols](#symbols)
- [Iterators](#iterators)
- [Generators](#generators)
- [Promises](#promises)
- [Maps](#maps)
- [WeakMaps](#weakmaps)
- [Sets](#sets)
- [WeakSets](#weaksets)
- [Proxies](#proxies)
- [Reflection](#reflection)
- [`Number`](#number)
- [`Math`](#math)
- [`Array`](#array)
- [`Object`](#object)
- [Strings e Unicode](#strings-e-unicode)
- [Modules](#modules)

Apologies about that long table of contents, and here we go.

# Introdução

- ES6 -- também conhecido como Harmony, `es-next` e ES2015 -- é a especificação mais recente finalizada da linguagem
- A especificação do ES6 foi finalizada em **Junho de 2015**, _(daí o nome ES2015)_
- Versões futuras da especificação seguirão o padrão `ES[ano]`, exemplo ES2016 para ES7
  - **Cronograma anual de lançamento**, features that don't make the cut take the next train
  - Since ES6 pre-dates that decision, most of us still call it ES6
  - Começando com ES2016 (ES7), nos devemos iniciar usando o padrão`ES[ano]` para referenciar versões mais recentes
  - Top reason for naming scheme is to pressure browser vendors into quickly implementing newest features
  - A maior razão para () é que fabricantes dos browsers tem pressionado para implementar rapidamente novas funcionalidades

<sup>[(back to table of contents)](#table-of-contents)</sup>

# Tooling

- Para se ter o ES6 trabalhando nos dias de hoje, você precisara de um _transpiler_ **JavaScript-to-JavaScript**
- Transpilers vieram pra ficar
  - They allow you to compile code in the latest version into older versions of the language
  - Suporte dos navegadores ficam melhores,  transpile ES2016 and ES2017 into ES6 and beyond
  - Vamos precisamos de uma funcionalidade melhor de source mapping
  - They're the most reliable way to run ES6 source code in production today _(although browsers get ES5)_
- Babel _(a transpiler)_ has a killer feature: **human-readable output**
- Uso do [`babel`][1] para transpilar ES6 em ES5 para construções estaticas
- Uso do [`babelify`][2] to incorporate `babel` em seu [Gulp, Grunt, ou `npm run`][26] no processo de construção
- Uso do Node.js `v4.x.x` ou ve maiores or greater as they have decent ES6 support baked in, thanks to `v8`
- Use `babel-node` with any version of `node`, as it transpiles modules into ES5
- Babel has a thriving ecosystem that already supports some of ES2016 and has plugin support
- Leia [A Brief History of ES6 Tooling][25]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# Assignment Destructuring

- `var {foo} = pony` é equivalente a `var foo = pony.foo`
- `var {foo: baz} = pony` é equivalente a `var baz = pony.foo`
- Você pode fornecer valores padrões, `var {foo='bar'} = baz` yields `foo: 'bar'` if `baz.foo` é `undefined`
- You can pull as many properties as you like, aliased or not
  - `var {foo, bar: baz} = {foo: 0, bar: 1}` retorna `foo: 0` and `baz: 1`
- Você pode ir mais fundo. `var {foo: {bar}} = { foo: { bar: 'baz' } }` retorna `bar: 'baz'`
- You can alias that too. `var {foo: {bar: deep}} = { foo: { bar: 'baz' } }` gets you `deep: 'baz'`
- Properties that aren't found yield `undefined` as usual, e.g: `var {foo} = {}`
- Deeply nested properties that aren't found yield an error, e.g: `var {foo: {bar}} = {}`
- It also works for arrays, `var [a, b] = [0, 1]` yields `a: 0` and `b: 1`
- Você pode pular itens em um array, `var [a, , b] = [0, 1, 2]`, getting `a: 0` and `b: 2`
- You can swap without an _"aux"_ variable, `[a, b] = [b, a]`
- Você tambem pode usar o destructuring nos parametros da função
  - Atribuir valores padrão como `function foo (bar=2) {}`
  - Those defaults can be objects, too `function foo (bar={ a: 1, b: 2 }) {}`
  - Destructure `bar` completely, like `function foo ({ a=1, b=2 }) {}`
  - Padrão para um objeto vazio se nada for fornecido, como `function foo ({ a=1, b=2 } = {}) {}`
- Leia [ES6 JavaScript Destructuring in Depth][3]

<sup>[(voltar ao indice de conteudo)](#indice-do-conteudo)</sup>

# Spread Operator e Rest Parameters

- Rest parameters é o melhor `arguments`
  - Você declara na assinatura do metodo como `function foo (...everything) {}`
  - `everything` é um array com todos os parametros passados para `foo`
  - Você pode informar alguns parametros antes `...everything`, como `function foo (bar, ...rest) {}`
  - Named parameters are excluded from `...rest`
  - `...rest` must be the last parameter in the list
- Spread operator is better than magic, also denoted with `...` syntax
  - Avoids `.apply` when calling methods, `fn(...[1, 2, 3])` is equivalent to `fn(1, 2, 3)`
  - Fácil concatenação `[1, 2, ...[3, 4, 5], 6, 7]`
  - Casts array-likes or iterables into an array, e.g `[...document.querySelectorAll('img')]`
  - Useful when [destructuring](#assignment-destructuring) too, `[a, , ...rest] = [1, 2, 3, 4, 5]` yields `a: 1` and `rest: [3, 4, 5]`
  - Faça sem problemas `new` + `.apply`, `new Date(...[2015, 31, 8])`
- Leia [ES6 Spread and Butter in Depth][6]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# Arrow Functions

- Maneira para se declarar uma função `param => returnValue`
- Useful when doing functional stuff like `[1, 2].map(x => x * 2)`
- Several flavors are available, might take you some getting used to
  - `p1 => expr` bom para um unico parametro
  - `p1 => expr` has an implicit `return` statement for the provided `expr` expression
  - Para retornar um objeto implicitamente, usamos o parenteses `() => ({ foo: 'bar' })` ou você tera um **erro**
  - Parenteses são obrigatórios quando você tem zero, dois ou mais parametros, `() => expr` ou `(p1, p2) => expr`
  - Brackets in the right-hand side represent a code block that can have multiple statements, `() => {}`
  - When using a code block, there's no implicit `return`, you'll have to provide it -- `() => { return 'foo' }`
- You can't name arrow functions statically, mas agora em tempo de execução estão muito melhores em inferir nomes para maioria dos métodos.
- Arrow functions are bound to their lexical scope
  - `this` é o mesmo que `this` context as in the parent scope
  - `this` não pode ser modificada por `.call`, `.apply`, ou métodos similares a _"reflection"-type_
- Leia [ES6 Arrow Functions in Depth][5]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# Template Literals

- Você pode declarar strings com `` ` `` (backticks),  além do `"` e `'`
- Strings wrapped in backticks are _template literals_
- Template literals can be multiline
- Template literals permite a interpolação como `` `ponyfoo.com is ${rating}` `` onde `rating` é uma variavel
- You can use any valid JavaScript expressions in the interpolation, such as `` `${2 * 3}` `` or `` `${foo()}` ``
- You can use tagged templates to change how expressions are interpolated
  - Adiciona um prefixo `fn` para ``fn`foo, ${bar} e ${baz}` ``
  - `fn` é chamada uma vez com `template, ...expressions`
  - `template` é `['foo, ', ' and ', '']` e `expressions` é `[bar, baz]`
  - O resultado de `fn` torna-se o valor do template literal
  - Possíveis casos de uso incluem a limpeza de entrada de expressões, analise de parametro e etc.
- Template literals are almost strictly better than strings wrapped in single or double quotes
- Leia [ES6 Template Literals in Depth][4]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# Object Literals

- Instead of `{ foo: foo }`, you can just do `{ foo }` -- known as a _property value shorthand_
- Computed property names, `{ [prefix + 'Foo']: 'bar'  }`, where `prefix: 'moz'`, yields `{ mozFoo: 'bar' }`
- Você não pode combinar nomes de propriedades computadas e valores de propriedade shorthands, `{ [foo] }` é inválido
- Definições de métodos em um objeto literal podem ser declaradas usando uma alternative, more terse syntax, `{ foo () {} }`
- Veja também a seção [`Object`](#object) section
- Leia [ES6 Object Literal Features in Depth][7]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# Classes

- Not _"traditional"_ classes, syntax sugar on top of prototypal inheritance
- Syntax similar para declaring objects, `class Foo {}`
- Instance methods _-- `new Foo().bar` --_ are declared using the short [object literal](#object-literals) syntax, `class Foo { bar () {} }`
- Métodos estáticos _-- `Foo.isPonyFoo()` --_  precisa de um palavra-chave para o prefixo `static`, `class Foo { static isPonyFoo () {} }`
- Constructor method `class Foo { constructor () { /* initialize instance */ } }`
- Prototypal inheritance with a simple syntax `class PonyFoo extends Foo {}`
- Leia [ES6 Classes in Depth][8]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# Let e Const

- `let` e `const` são alternativas de uso do `var` na declaração de variaveis.
- `let` is block-scoped instead of lexically scoped to a `function`
- `let` is [hoisted][27] to the top of the block, while `var` declarations are hoisted to top of the function
- "Temporal Dead Zone" -- TDZ for short
  - Começa no inicio do bloco, onde `let foo` foi declarado
  - Termina onde o `let foo` statement was placed in user code _(hoisiting is irrelevant here)_
  - Attempts to access or assign to `foo` within the TDZ _(before the `let foo` statement is reached)_ result in an error
  - Ajuda a previnir erros misteriosos onde uma variavel é manipulada antes de sua declaração ser atingida reached
- `const` é também um bloco do escopo, hoisted, and constrained by TDZ semantics
- `const` variaveis devem ser declaradas usando um inicializador, `const foo = 'bar'`
- Assigning to `const` after initialization fails silently (or **loudly** _-- with an exception --_ under strict mode)
- `const` variaveis não fazem o valor atribuído imutavel
  - `const foo = { bar: 'baz' }` means `foo` will always reference the right-hand side object
  - `const foo = { bar: 'baz' }; foo.bar = 'boo'` won't throw
- Declaration of a variable by the same name will throw
- Meant to fix mistakes where you reassign a variable and lose a reference that was passed along somewhere else
- In ES6, **functions are block scoped**
  - Prevents leaking block-scoped secrets through hoisting, `{ let _foo = 'secret', bar = () => _foo; }`
  - Na maioria das situações, não quebra o código do usuário, e and typically what you wanted anyways
- Leia [ES6 Let, Const and the “Temporal Dead Zone” (TDZ) in Depth][9]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# Symbols

- Um novo tipo primitivo em ES6
- Você pode criar seus proprios simbolos usando o `var symbol = Symbol()`
- Você pode adicionar uma descrição para o debugging purposes, como `Symbol('ponyfoo')`
- Symbols são unicos e umutaveis. `Symbol()`, `Symbol()`, `Symbol('foo')` e `Symbol('foo')` são todos diferentes
- Symbols são o tipo `symbol`, thus: `typeof Symbol() === 'symbol'`
- Você também pode criar simbolos globais com `Symbol.for(key)`
  - If a symbol with the provided `key` already existed, you get that one back
  - Otherwise, a new symbol is created, using `key` as its description as well
  - `Symbol.keyFor(symbol)` is the inverse function, taking a `symbol` and returning its `key`
  - Global symbols are **as global as it gets**, or _cross-realm_. Single registry used to look up these symbols across the runtime
    - `window` context
    - `eval` context
    - `<iframe>` context, `Symbol.for('foo') === iframe.contentWindow.Symbol.for('foo')`
- There's also "well-known" symbols
  - Not on the global registry, accessible through `Symbol[name]`, e.g: `Symbol.iterator`
  - Cross-realm, meaning `Symbol.iterator === iframe.contentWindow.Symbol.iterator`
  - Usado por uma especificação para definição de protocolos, tais como o [_iterable_ protocol](#iterators) sobre o `Symbol.iterator`
  - They're not **actually well-known** -- in colloquial terms
 - Iterating over symbol properties is hard, but not impossible and definitely not private
  - Symbols são invisiveis para todos os metodos pre-ES6 "reflection"
  - Symbols são acessiveis atraves do `Object.getOwnPropertySymbols`
  - You won't stumble upon them but you **will** find them if _actively looking_
- Leia [ES6 Symbols in Depth][12]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# Iterators

- Iterator e Iterable protocol define a forma de interação sobre qualquer objeto, não apenas em arrays e arrays-likes
 Iterator e iterable protocol define how to iterate over any object, not just arrays and array-likes
- A well-known `Symbol` is used to assign an iterator to any object
- `var foo = { [Symbol.iterator]: iterable}`, or `foo[Symbol.iterator] = iterable`
- O `iterable` é um método que retorna um `iterator` que possui um método `next`
- O método `next` retorna objetos com duas propriedades, `value` e `done`
  - A propriedade `value` indica que o valor atual da sequencia está sendo iterado
  - A propriedade `done` indica se há mais itens para ser iterados
- Objects that have a `[Symbol.iterator]` value are _iterable_, because they subscribe to the iterable protocol
- Some built-ins like `Array`, `String`, or `arguments` -- and `NodeList` in browsers -- are iterable by default in ES6
- Iterable objects can be looped over with `for..of`, such as `for (let el of document.querySelectorAll('a'))`
- Iterable objects can be synthesized using the spread operator, like `[...document.querySelectorAll('a')]`
- Você também pode usar o `Array.from(document.querySelectorAll('a'))` para sintetizar uma sequencia iteravel em um array
- Iterators são _lazy_, and those that produce an infinite sequence still can lead to valid programs
- Be careful not to attempt to synthesize an infinite sequence with `...` or `Array.from` as that **will** cause an infinite loop
- Leia [ES6 Iterators in Depth][10]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# Generators

- Funções geradoras são uma tipo espcial de _iterator_ que podem ser declarados usando a syntax `function* generator () {}`
- Funções geradoras usam `yield` para emitir uma sequencia de elemento
- Funções geradoras também podem usar `yield*` para delegar outra função geradora _-- ou qualquer objeto iteravel_
- Funções geradoras returna um objeto gerador que adere tanto no _iterable_ quanto no _iterator_ protocols
  - Given `g = generator()`, `g` adere para um iterable protocol por conta de que o `g[Symbol.iterator]` é um método
  - Given `g = generator()`, `g` adere para um iterable protocol por conta de que o `g.next` é um método
  - The iterator for a generator object `g` is the generator itself: `g[Symbol.iterator]() === g`
- Pull values using `Array.from(g)`, `[...g]`, `for (let item of g)`, or just calling `g.next()`
- Generator function execution is suspended, remembering the last position, in four different cases
  - A `yield` expression returning the next value in the sequence
  - A `return` statement returning the last value in the sequence
  - A `throw` statement halts execution in the generator entirely
  - Reaching the end of the generator function signals `{ done: true }`
- Once the `g` sequence has ended, `g.next()` simply returns `{ done: true }` and has no effect
- It's easy to make asynchronous flows feel synchronous
  - Take user-provided generator function
  - User code is suspended while asynchronous operations take place
  - Call `g.next()`, unsuspending execution in user code
- Leia [ES6 Generators in Depth][11]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# Promises

- Seguindo a especificação [`Promises/A+`][35], was widely implemented in the wild before ES6 was standarized _(e.g [`bluebird`][34])_
- Promises behave like a tree. Add branches with `p.then(handler)` and `p.catch(handler)`
- Cria uma nova promisse `p` com `new Promise((resolve, reject) => { /* resolver */ })`
  - The `resolve(value)` callback will fulfill the promise with the provided `value`
  - The `reject(reason)` callback will reject `p` with a `reason` error
  - You can call those methods asynchronously, blocking deeper branches of the promise tree
- Cada chamada para `p.then` e `p.catch` creates another promise that's blocked on `p` being settled
- Promises start out in _pending_ state and are **settled** when they're either _fulfilled_ or _rejected_
- Promises só podem ser resolvidas uma por vez, and then they're settled. Settled promises unblock deeper branches
- You can tack as many promises as you want onto as many branches as you need
- Each branch will execute either `.then` handlers or `.catch` handlers, never both
- A `.then` callback can transform the result of the previous branch by returning a value
- A `.then` callback can block on another promise by returning it
- `p.catch(fn).catch(fn)` won't do what you want -- unless what you wanted is to catch errors in the error handler
- [`Promise.resolve(value)`](https://ponyfoo.com/articles/es6-promises-in-depth#using-promiseresolve-and-promisereject) cria uma promise creates a promise that's fulfilled with the provided `value`
- [`Promise.reject(reason)`](https://ponyfoo.com/articles/es6-promises-in-depth#using-promiseresolve-and-promisereject) cria uma promise que é rejeitada como previsto `reason`
- [`Promise.all(...promises)`](https://ponyfoo.com/articles/es6-promises-in-depth#leveraging-promiseall-and-promiserace) creates a promise that settles when all `...promises` are fulfilled or 1 of them is rejected
- [`Promise.race(...promises)`](https://ponyfoo.com/articles/es6-promises-in-depth#leveraging-promiseall-and-promiserace) creates a promise that settles as soon as 1 of `...promises` is settled
- Use [Promisees][33] -- the promise visualization playground -- to better understand promises
- Leia [ES6 Promises in Depth][32]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# Maps

- A replacement to the common pattern of creating a hash-map using plain JavaScript objects
  - Avoids security issues with user-provided keys
  - Allows keys to be arbitrary values, you can even use DOM elements or functions as the `key` to an entry
- `Map` adheres to _[iterable](#iterators)_ protocol
- Crie um `map` usando `new Map()`
- Inicializa um mapa com um `iterable` como `[[key1, value1], [key2, value2]]` em `new Map(iterable)`
- Uso de `map.set(key, value)` para adicionar entradas
- Uso de `map.get(key)` para obter uma entrada
- Verifique uma `key` usando `map.has(key)`
- Remova entradas com `map.delete(key)`
- Iterate over `map` com `for (let [key, value] of map)`, the spread operator, `Array.from`, etc
- Leia [ES6 Maps in Depth][13]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# WeakMaps

- Semelhante ao `Map`, mas são diferentes
- `WeakMap` isn't iterable, so you don't get enumeration methods like `.forEach`, `.clear`, and others you had in `Map`
- `WeakMap` keys must be reference types. You can't use value types like symbols, numbers, or strings as keys
- `WeakMap` entries with a `key` that's the only reference to the referenced variable are subject to garbage collection
- That last point means `WeakMap` is great at keeping around metadata for objects, while those objects are still in use
- You avoid memory leaks, without manual reference counting -- think of `WeakMap` as [`IDisposable`][28] in .NET
- Leia [ES6 WeakMaps in Depth][29]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# Sets

- Similar ao `Map`, mas diferentes
- `Set` não possuem chaves, somente valores
- `set.set(value)` doesn't look right, so we have `set.add(value)` instead
- Sets não podem ter valores duplicados porque os valores são também usados em chaves
- Leia [ES6 Sets in Depth][30]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# WeakSets

- `WeakSet` is sort of a cross-breed between `Set` and `WeakMap`
-  Um `WeakSet` is a set that can't be iterated and doesn't have enumeration methods
- `WeakSet` valores devem ser do mesmo tipo da referencia
- `WeakSet` may be useful for a metadata table indicating whether a reference is actively in use or not
- Leia [ES6 WeakSets in Depth][31]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# Proxies

- Proxies são criados com `new Proxy(target, handler)`, onde `target` é qualquer objeto e o `handler` é a configuração
- O comportamento padrão de um `proxy` acts as a passthrough to the underlying `target` object
- Handlers determina como o `target` adjacente object is accessed on top of regular object property access semantics
- You pass off references to `proxy` and retain strict control over how `target` can be interacted with
- Handlers também são conhecidos como armadilhas, termos usados alternadamentes
- Você pode criar um **revocable** proxies com `Proxy.revocable(target, handler)`
  - That method returns an object with `proxy` and `revoke` properties
  - You could [destructure](#destructuring) `var {proxy, revoke} = Proxy.revocable(target, handler)` for convenience
  - Você pode configurar o `proxy` all the same as with `new Proxy(target, handler)`
  - Depois de chamado `revoke()`, o `proxy` will **throw** on _any operation_, making it convenient when you can't trust consumers
- [`get`](https://ponyfoo.com/articles/es6-proxies-in-depth#get) -- armadilhas `proxy.prop` e `proxy['prop']`
- [`set`](https://ponyfoo.com/articles/es6-proxies-in-depth#set) -- armadilhas `proxy.prop = value` e `proxy['prop'] = value`
- [`has`](https://ponyfoo.com/articles/es6-proxy-traps-in-depth#has) -- armadilhas de operador `in`
- [`deleteProperty`](https://ponyfoo.com/articles/es6-proxy-traps-in-depth#deleteproperty) -- armadilhas para operador `delete`
- [`defineProperty`](https://ponyfoo.com/articles/es6-proxy-traps-in-depth#defineproperty) -- armadilhas para `Object.defineProperty` and declarative alternatives
- [`enumerate`](https://ponyfoo.com/articles/es6-proxy-traps-in-depth#enumerate) -- traps `for..in` loops
- [`ownKeys`](https://ponyfoo.com/articles/es6-proxy-traps-in-depth#ownkeys) -- traps `Object.keys` and related methods
- [`apply`](https://ponyfoo.com/articles/es6-proxy-traps-in-depth#apply) -- traps _function calls_
- [`construct`](https://ponyfoo.com/articles/morees6-proxy-traps-in-depth#construct) -- armadilhas usando o operador `new`
- [`getPrototypeOf`](https://ponyfoo.com/articles/morees6-proxy-traps-in-depth#getprototypeof) -- traps chamadas internas para `[[GetPrototypeOf]]`
- [`setPrototypeOf`](https://ponyfoo.com/articles/morees6-proxy-traps-in-depth#setprototypeof) -- traps  chamadas para `Object.setPrototypeOf`
- [`isExtensible`](https://ponyfoo.com/articles/morees6-proxy-traps-in-depth#isextensible) -- traps chamadas para `Object.isExtensible`
- [`preventExtensions`](https://ponyfoo.com/articles/morees6-proxy-traps-in-depth#preventextensions) -- traps chamadas para `Object.preventExtensions`
- [`getOwnPropertyDescriptor`](https://ponyfoo.com/articles/morees6-proxy-traps-in-depth#getownpropertydescriptor) -- traps chamadas para `Object.getOwnPropertyDescriptor`
- Leia [ES6 Proxies in Depth][15]
- Leia [ES6 Proxy Traps in Depth][16]
- Leia [More ES6 Proxy Traps in Depth][17]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# Reflection

- `Reflection` é uma static construidas a partir do (Pensar em `Math`) em ES6
- `Reflection` métodos internos sensiveis, e.g `Reflect.defineProperty` retorna um boolean instead of throwing
- There's a `Reflection` method for each proxy trap handler, and they represent the default behavior of each trap
- Going forward, new reflection methods in the same vein as `Object.keys` will be placed in the `Reflection` namespace
- Leia [ES6 Reflection in Depth][18]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# `Number`

- Use o prefixo `0b` para binário, and `0o` prefix for octal integer literals
- `Number.isNaN` e `Number.isFinite` are like their global namesakes, except that they *don't* coerce input to `Number`
- `Number.parseInt` e `Number.parseFloat` are exactly the same as their global namesakes
- `Number.isInteger` verifica se a entrada é um valor `Number` que não contenha decimais
- `Number.EPSILON` helps figure out negligible differences between two numbers -- e.g. `0.1 + 0.2` and `0.3`
- `Number.MAX_SAFE_INTEGER` is the largest integer that can be safely and precisely represented in JavaScript
- `Number.MIN_SAFE_INTEGER` is the smallest integer that can be safely and precisely represented in JavaScript
- `Number.isSafeInteger` checks whether an integer is within those bounds, able to be represented safely and precisely
- Leia [ES6 `Number` Improvements in Depth][19]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# `Math`

- [`Math.sign`](https://ponyfoo.com/articles/es6-math-additions-in-depth#mathsign) -- sign função de um numero
- [`Math.trunc`](https://ponyfoo.com/articles/es6-math-additions-in-depth#mathtrunc) -- parte inteira de um numero
- [`Math.cbrt`](https://ponyfoo.com/articles/es6-math-additions-in-depth#mathcbrt) -- cubic root of value, or <code class='md-code md-code-inline'>∛‾value</code>
- [`Math.expm1`](https://ponyfoo.com/articles/es6-math-additions-in-depth#mathexpm1) -- `e` to the `value` minus `1`, or <code class='md-code md-code-inline'>e<sup>value</sup> - 1</code>
- [`Math.log1p`](https://ponyfoo.com/articles/es6-math-additions-in-depth#mathlog1p) -- natural logarithm of `value + 1`, or <code class='md-code md-code-inline'><em>ln</em>(value + 1)</code>
- [`Math.log10`](https://ponyfoo.com/articles/es6-math-additions-in-depth#mathlog10) -- base 10 logarithm of `value`, or <code class='md-code md-code-inline'><em>log</em><sub>10</sub>(value)</code>
- [`Math.log2`](https://ponyfoo.com/articles/es6-math-additions-in-depth#mathlog2) -- base 2 logarithm of `value`, or <code class='md-code md-code-inline'><em>log</em><sub>2</sub>(value)</code>
- [`Math.sinh`](https://ponyfoo.com/articles/es6-math-additions-in-depth#mathsinh) -- hyperbolic sine of a number
- [`Math.cosh`](https://ponyfoo.com/articles/es6-math-additions-in-depth#mathcosh) -- hyperbolic cosine of a number
- [`Math.tanh`](https://ponyfoo.com/articles/es6-math-additions-in-depth#mathtanh) -- hyperbolic tangent of a number
- [`Math.asinh`](https://ponyfoo.com/articles/es6-math-additions-in-depth#mathasinh) -- hyperbolic arc-sine of a number
- [`Math.acosh`](https://ponyfoo.com/articles/es6-math-additions-in-depth#mathacosh) -- hyperbolic arc-cosine of a number
- [`Math.atanh`](https://ponyfoo.com/articles/es6-math-additions-in-depth#mathatanh) -- hyperbolic arc-tangent of a number
- [`Math.hypot`](https://ponyfoo.com/articles/es6-math-additions-in-depth#mathhypot) -- square root of the sum of squares
- [`Math.clz32`](https://ponyfoo.com/articles/es6-math-additions-in-depth#mathclz32) -- leading zero bits in the 32-bit representation of a number
- [`Math.imul`](https://ponyfoo.com/articles/es6-math-additions-in-depth#mathimul) -- _C-like_ 32-bit multiplication
- [`Math.fround`](https://ponyfoo.com/articles/es6-math-additions-in-depth#mathfround) -- nearest single-precision float representation of a number
- Leia [ES6 `Math` Additions in Depth][20]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# `Array`

- [`Array.from`](https://ponyfoo.com/articles/es6-array-extensions-in-depth#arrayfrom) -- create `Array` instances from arraylike objects like `arguments` or iterables
- [`Array.of`](https://ponyfoo.com/articles/es6-array-extensions-in-depth#arrayof) -- similar to `new Array(...items)`, but without special cases
- [`Array.prototype.copyWithin`](https://ponyfoo.com/articles/es6-array-extensions-in-depth#arrayprototypecopywithin) -- copies a sequence of array elements into somewhere else in the array
- [`Array.prototype.fill`](https://ponyfoo.com/articles/es6-array-extensions-in-depth#arrayprototypefill) -- fills all elements of an existing array with the provided value
- [`Array.prototype.find`](https://ponyfoo.com/articles/es6-array-extensions-in-depth#arrayprototypefind) -- returns the first item to satisfy a callback
- [`Array.prototype.findIndex`](https://ponyfoo.com/articles/es6-array-extensions-in-depth#arrayprototypefindindex) -- returns the index of the first item to satisfy a callback
- [`Array.prototype.keys`](https://ponyfoo.com/articles/es6-array-extensions-in-depth#arrayprototypekeys) -- returns an iterator that yields a sequence holding the keys for the array
- [`Array.prototype.values`](https://ponyfoo.com/articles/es6-array-extensions-in-depth#arrayprototypevalues) -- returns an iterator that yields a sequence holding the values for the array
- [`Array.prototype.entries`](https://ponyfoo.com/articles/es6-array-extensions-in-depth#arrayprototypeentries) -- returns an iterator that yields a sequence holding key value pairs for the array
- [`Array.prototype[Symbol.iterator]`](https://ponyfoo.com/articles/es6-array-extensions-in-depth#arrayprototype-symboliterator) -- exactly the same as the [`Array.prototype.values`](https://ponyfoo.com/articles/es6-array-extensions-in-depth#arrayprototypevalues) method
- Leia [ES6 `Array` Extensions in Depth][21]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# `Object`

- [`Object.assign`](https://ponyfoo.com/articles/es6-object-changes-in-depth#objectassign) -- recursive shallow overwrite for properties from `target, ...objects`
- [`Object.is`](https://ponyfoo.com/articles/es6-object-changes-in-depth#objectis) -- like using the `===` operator, but `true` for `NaN` vs `NaN`, and `false` for `+0` vs `-0`
- [`Object.getOwnPropertySymbols`](https://ponyfoo.com/articles/es6-object-changes-in-depth#objectgetownpropertysymbols) -- returns all own property symbols found on an object
- [`Object.setPrototypeOf`](https://ponyfoo.com/articles/es6-object-changes-in-depth#objectsetprototypeof) -- changes prototype. Equivalent to `Object.prototype.__proto__` setter
- See also [Object Literals](#object-literals) section
- Leia [ES6 `Object` Changes in Depth][22]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# Strings and Unicode

- Manipulação de Strings
  - [`String.prototype.startsWith`](https://ponyfoo.com/articles/es6-strings-and-unicode-in-depth#stringprototypestartswith) --  se a string começa com o `valor`
  - [`String.prototype.endsWith`](https://ponyfoo.com/articles/es6-strings-and-unicode-in-depth#stringprototypeendswith) -- se a string termina em `valor`
  - [`String.prototype.includes`](https://ponyfoo.com/articles/es6-strings-and-unicode-in-depth#stringprototypeincludes) -- se a string contém qualquer `valor`
  - [`String.prototype.repeat`](https://ponyfoo.com/articles/es6-strings-and-unicode-in-depth#stringprototyperepeat) -- retorna o `amount` da string repetidamente
  - [`String.prototype[Symbol.iterator]`](https://ponyfoo.com/articles/es6-strings-and-unicode-in-depth#stringprototype-symboliterator) -- lets you iterate over a sequence of unicode code points _(not characters)_
- [Unicode](https://ponyfoo.com/articles/es6-strings-and-unicode-in-depth#unicode)
  - [`String.prototype.codePointAt`](https://ponyfoo.com/articles/es6-strings-and-unicode-in-depth#stringprototypecodepointat) -- representação numerica da base-10 em um ponto do codigo em uma determinada posição na string
  - [`String.fromCodePoint`](https://ponyfoo.com/articles/es6-strings-and-unicode-in-depth#stringfromcodepoint`) -- given `...codepoints`, returna uma string feita pela representação do unicode
  - [`String.prototype.normalize`](https://ponyfoo.com/articles/es6-strings-and-unicode-in-depth#stringprototypenormalize) -- retorna uma versão normalizada da representação unicode da strings
- Leia [ES6 Strings and Unicode Additions in Depth][23]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

# Modules

- [Strict Mode](https://ponyfoo.com/articles/es6-modules-in-depth#strict-mode) Ativado por padrão no sistema de módulo do ES6
- ES6 modules são arquivos de [`export`](https://ponyfoo.com/articles/es6-modules-in-depth#export) de uma API
- [`export default value`](https://ponyfoo.com/articles/es6-modules-in-depth#exporting-a-default-binding) exports a default binding
- [`export var foo = 'bar'`](https://ponyfoo.com/articles/es6-modules-in-depth#named-exports) exports a named binding
- exports são chamadas que [can be changed](https://ponyfoo.com/articles/es6-modules-in-depth#bindings-not-values) at any time from the module that's exporting them
- `export { foo, bar }` exports [a list of named exports](https://ponyfoo.com/articles/es6-modules-in-depth#exporting-lists)
- `export { foo as ponyfoo }` aliases the export to be referenced as `ponyfoo` instead
- `export { foo as default }` marks the named export as the default export
- As [a best practice](https://ponyfoo.com/articles/es6-modules-in-depth#best-practices-and-export), `export default api` at the end of all your modules, where `api` is an object, avoids confusion
- Module loading is implementation-specific, allows interoperation with CommonJS
- [`import 'foo'`](https://ponyfoo.com/articles/es6-modules-in-depth#import) loads the `foo` module into the current module
- [`import foo from 'ponyfoo'`](https://ponyfoo.com/articles/es6-modules-in-depth#importing-default-exports) assigns the default export of `ponyfoo` to a local `foo` variable
- [`import {foo, bar} from 'baz'`](https://ponyfoo.com/articles/es6-modules-in-depth#importing-named-exports) imports named exports `foo` and `bar` from the `baz` module
- `import {foo as bar} from 'baz'` imports named export `foo` but aliased as a `bar` variable
- `import {default} from 'foo'` also imports the default export
- `import {default as bar} from 'foo'` imports the default export aliased as `bar`
- `import foo, {bar, baz} from 'foo'` mixes default `foo` with named exports `bar` and `baz` in one declaration
- [`import * as foo from 'foo'`](https://ponyfoo.com/articles/es6-modules-in-depth#import-all-the-things) imports the namespace object
  - Contém todos exports nomeados em `foo[name]`
  - Contém por padrão Contains the default export in `foo.default`, se um padrão foi declarada no modulo
- Leia [ES6 Modules Additions in Depth][24]

<sup>[(voltar ao indice de conteudos)](#indice-do-conteudo)</sup>

Time for a bullet point detox. Then again, I _did warn you_ to read the [article series][36] instead. Don't forget to subscribe and maybe even [contribute to keep Pony Foo alive][37]. Also, did you try the [Konami code][38] just yet?

[1]: http://babeljs.io/ "Babel JavaScript Compiler"
[2]: https://github.com/babel/babelify "babel/babelify on GitHub"
[3]: https://ponyfoo.com/articles/es6-destructuring-in-depth "ES6 Destructuring in Depth on Pony Foo"
[4]: https://ponyfoo.com/articles/es6-template-strings-in-depth "ES6 Template Literals on Pony Foo"
[5]: https://ponyfoo.com/articles/es6-arrow-functions-in-depth "ES6 Arrow Functions on Pony Foo"
[6]: https://ponyfoo.com/articles/es6-spread-and-butter-in-depth "ES6 Spread and Butter on Pony Foo"
[7]: https://ponyfoo.com/articles/es6-object-literal-features-in-depth "ES6 Object Literal Features in Depth on Pony Foo"
[8]: https://ponyfoo.com/articles/es6-classes-in-depth "ES6 Classes in Depth on Pony Foo"
[9]: https://ponyfoo.com/articles/es6-let-const-and-temporal-dead-zone-in-depth "ES6 Let, Const, and the 'Temporal Dead Zone' (TDZ) in Depth on Pony Foo"
[10]: https://ponyfoo.com/articles/es6-iterators-in-depth "ES6 Iterators in Depth on Pony Foo"
[11]: https://ponyfoo.com/articles/es6-generators-in-depth "ES6 Generators in Depth on Pony Foo"
[12]: https://ponyfoo.com/articles/es6-symbols-in-depth "ES6 Symbols in Depth on Pony Foo"
[13]: https://ponyfoo.com/articles/es6-maps-in-depth "ES6 Maps in Depth on Pony Foo"
[14]: https://ponyfoo.com/articles/es6-weakmaps-sets-and-weaksets-in-depth "ES6 WeakMaps, Sets, and WeakSets in Depth on Pony Foo"
[15]: https://ponyfoo.com/articles/es6-proxies-in-depth "ES6 Proxies in Depth on Pony Foo"
[16]: https://ponyfoo.com/articles/es6-proxy-traps-in-depth "ES6 Proxy Traps in Depth on Pony Foo"
[17]: https://ponyfoo.com/articles/more-es6-proxy-traps-in-depth "More ES6 Proxy Traps in Depth on Pony Foo"
[18]: https://ponyfoo.com/articles/es6-reflection-in-depth "ES6 Reflection in Depth on Pony Foo"
[19]: https://ponyfoo.com/articles/es6-number-improvements-in-depth "ES6 Number Improvements in Depth on Pony Foo"
[20]: https://ponyfoo.com/articles/es6-math-additions-in-depth "ES6 Math Additions in Depth on Pony Foo"
[21]: https://ponyfoo.com/articles/es6-array-extensions-in-depth "ES6 Array Extensions in Depth on Pony Foo"
[22]: https://ponyfoo.com/articles/es6-object-changes-in-depth "ES6 Object Changes in Depth on Pony Foo"
[23]: https://ponyfoo.com/articles/es6-strings-and-unicode-in-depth "ES6 Strings (and Unicode, ❤) in Depth"
[24]: https://ponyfoo.com/articles/es6-modules-in-depth "ES6 Modules in Depth on Pony Foo"
[25]: https://ponyfoo.com/articles/a-brief-history-of-es6-tooling "A Brief History of ES6 Tooling on Pony Foo"
[26]: https://ponyfoo.com/articles/gulp-grunt-whatever "Gulp, Grunt, Whatever on Pony Foo"
[27]: https://ponyfoo.com/articles/javascript-variable-hoisting "JavaScript Variable Hoisting on Pony Foo"
[28]: https://msdn.microsoft.com/en-us/library/system.idisposable%28v=vs.110%29.aspx?f=255&MSPPError=-2147217396 "IDisposable on MSDN"
[29]: https://ponyfoo.com/articles/es6-weakmaps-sets-and-weaksets-in-depth#es6-weakmaps "ES6 WeakMaps, Sets, and WeakSets in Depth on Pony Foo"
[30]: https://ponyfoo.com/articles/es6-weakmaps-sets-and-weaksets-in-depth#es6-sets "ES6 WeakMaps, Sets, and WeakSets in Depth on Pony Foo"
[31]: https://ponyfoo.com/articles/es6-weakmaps-sets-and-weaksets-in-depth#es6-weaksets "ES6 WeakMaps, Sets, and WeakSets in Depth on Pony Foo"
[32]: https://ponyfoo.com/articles/es6-promises-in-depth "ES6 Promises in Depth"
[33]: http://bevacqua.github.io/promisees/ "Promisees — Promise visualization playground for the adventurous"
[34]: https://github.com/petkaantonov/bluebird "petkaantonov/bluebird on GitHub"
[35]: https://promisesaplus.com/ "An open standard for sound, interoperable JavaScript promises"
[36]: https://ponyfoo.com/articles/tagged/es6-in-depth "ES6 in Depth on Pony Foo"
[37]: http://patreon.com/bevacqua "My Patreon Account"
[38]: https://en.wikipedia.org/wiki/Konami_Code
[39]: https://ponyfoo.com/articles/tagged/es6-in-depth "ES6 in Depth on Pony Foo"
[40]: https://i.imgur.com/tbKTICw.png