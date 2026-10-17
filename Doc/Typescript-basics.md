# TypeScript Basics

This guide starts with the building blocks used in a React application. TypeScript is JavaScript with optional type information. It checks many mistakes while you are writing code, then compiles to JavaScript that can run in a browser.

## JavaScript and TypeScript

TypeScript uses familiar JavaScript syntax. The main difference is that you can add types, which help catch incorrect values before the code runs.

JavaScript:

```js
let quantity = 2;

function calculateTotal(price, quantity) {
	return price * quantity;
}
```

TypeScript adds types to the variable and function:

```ts
let quantity: number = 2;

function calculateTotal(price: number, quantity: number): number {
	return price * quantity;
}
```

If you pass text where a number is expected, TypeScript reports an error while you are developing:

```ts
calculateTotal("twenty", 2); // Error: price must be a number
```

Loops, conditions, and most everyday JavaScript code look the same in TypeScript. Type annotations are used by TypeScript's checker and are removed when TypeScript compiles the code to JavaScript.

## 1. Variables

Use `const` when you do not need to assign a new value to a variable. Use `let` when you do. Avoid `var` in modern TypeScript because its older scoping rules can be confusing.

```ts
const storeName = "Corde Boheme";
let itemCount = 0;

itemCount = 3;
```

TypeScript can often work out a variable's type from its first value. This is called **type inference**. You can also write the type after the variable name using a colon:

```ts
const customerName: string = "Aman";
let cartTotal: number = 0;
let isSignedIn: boolean = false;
```

Common basic types:

- `string`: text, such as `"Handbag"`
- `number`: whole or decimal numbers, such as `3` or `19.99`
- `boolean`: `true` or `false`

TypeScript reports an error if you assign a value of the wrong type:

```ts
let quantity: number = 2;
// quantity = "two"; // Error: a string is not a number
```

## 2. Arrays and Objects

An array stores a list of values. Add `[]` after the type to describe what it contains:

```ts
const productNames: string[] = ["Tote bag", "Wallet", "Pouch"];
const prices: number[] = [49.99, 24.5, 15];
```

An object groups related values. Define its shape with a `type`:

```ts
type Product = {
	id: number;
	name: string;
	price: number;
	inStock: boolean;
};

const product: Product = {
	id: 1,
	name: "Tote bag",
	price: 49.99,
	inStock: true,
};
```

The `Product` type makes clear which fields a product must have and what values they can contain.

## 3. Conditions

Use `if` and `else` to choose which code runs. Comparisons include `===` (equal), `!==` (not equal), `>` (greater than), and `<` (less than).

```ts
const cartTotal = 85;

if (cartTotal >= 50) {
	console.log("Free shipping");
} else {
	console.log("Shipping fee applies");
}
```

Use `===` to compare values rather than `=`. A single `=` assigns a value.

## 4. Loops

Loops repeat code. A `for...of` loop is useful when you want to do something for each value in an array:

```ts
const productNames = ["Tote bag", "Wallet", "Pouch"];

for (const productName of productNames) {
	console.log(productName);
}
```

A regular `for` loop is useful when you need the item's position, called its index:

```ts
for (let index = 0; index < productNames.length; index++) {
	console.log(index, productNames[index]);
}
```

In React, lists are often displayed with the array's `map` method instead of a loop statement. See [React Components and JSX](React-learning.md#rendering-a-list) for an example.

## 5. Functions

A function is a reusable block of code. Give its inputs types and, when helpful, state the type of value it returns:

```ts
function calculateTotal(price: number, quantity: number): number {
	return price * quantity;
}

const total = calculateTotal(24.5, 2);
```

Here, `price` and `quantity` are numbers, and `: number` after the parentheses says the function returns a number.

An arrow function is another common way to write a function:

```ts
const formatPrice = (price: number): string => {
	return `$${price.toFixed(2)}`;
};
```

Use `void` when a function does not return a value:

```ts
function showMessage(message: string): void {
	console.log(message);
}
```

## 6. Classes

A class is a blueprint for creating objects that hold data and have functions. In a class, the data is stored in **properties** (also called fields), and the functions are called **methods**. In frontend applications, object types are often enough, but classes are useful to understand when you encounter them.

```ts
class Product {
	name: string;
	price: number;
	quantity: number;

	constructor(name: string, price: number) {
		this.name = name;
		this.price = price;
		this.quantity = 0;
	}

	addToCart(): void {
		this.quantity += 1;
	}

	getTotal(): number {
		return this.price * this.quantity;
	}
}

const tote = new Product("Tote bag", 49.99);

console.log(tote.name); // Read a property: Tote bag
tote.addToCart(); // Call a method
tote.addToCart();
console.log(tote.quantity); // Read a property: 2
console.log(tote.getTotal()); // Call a method: 99.98
```

- `name`, `price`, and `quantity` are properties. Each property has a type.
- `constructor` runs when `new Product(...)` creates an instance and sets its starting values.
- `this` means the current instance. For example, `this.price` refers to this product's price.
- `addToCart()` and `getTotal()` are methods. Use parentheses to call a method, such as `tote.addToCart()`.
- Use a dot to read a property, such as `tote.name`.
- `: void` means `addToCart` does not return a value; `: number` means `getTotal` returns a number.

## Key Takeaways

- Use `const` by default and `let` when you need to reassign a variable; avoid `var`.
- Types such as `string`, `number`, and `boolean` describe what values can be stored.
- Arrays and object types describe collections and structured data.
- Conditions choose what code runs; loops repeat code.
- Functions accept typed inputs and can return typed values.
- Classes group data and methods behind a reusable blueprint.
