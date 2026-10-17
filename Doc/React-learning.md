# React Learning Guide

This guide introduces the React ideas you will use most often when building a website. The examples use React with TypeScript (`.tsx` files). React builds the part of an application that people see and interact with in their browser. It commonly gets and saves business data through APIs provided by a backend such as Spring Boot.

If you still need to create or run the project, follow the setup steps in [Basic Info Frontend](Basic-info-frontend.md). This guide focuses on what to learn once the project is running.

## 1. The React Mental Model

Think of a React page as a tree of small components. Each component receives input, then returns a description of what should appear on screen. When a value that the screen depends on changes, React updates the relevant UI.

For example, an online shop page could be arranged like this:

```text
App
└── ProductPage
    ├── ProductSearch
    └── ProductList
        └── ProductCard (one for each product)
```

You write the description; React takes care of updating the browser. In a Spring Boot application, the controller and service usually run on the server. React components run in the user's browser, so React is not a replacement for backend business rules or database access.

## 2. JSX: Describe the UI

JSX looks like HTML, but it is written inside JavaScript or TypeScript. Use curly braces to put a JavaScript value or expression into the UI.

```tsx
const productName = "Leather Tote";
const price = 79.99;

function ProductSummary() {
	return (
		<section className="product-summary">
			<h2>{productName}</h2>
			<p>${price.toFixed(2)}</p>
		</section>
	);
}
```

Useful JSX rules:

- Return one parent element. Use a fragment (`<>...</>`) if you do not want to add an HTML element.
- Use `className` instead of `class` for CSS classes.
- Close tags, including tags such as `<img />` and `<input />`.
- Put JavaScript expressions inside `{}`. For example, `{price * 2}`.

## 3. Components: Split the Screen into Parts

A component is a function whose name starts with a capital letter. It returns JSX, and you can use it like a custom HTML tag.

```tsx
function ProductTitle() {
	return <h2>Leather Tote</h2>;
}

function ProductPage() {
	return (
		<main>
			<ProductTitle />
			<p>Handmade leather bag</p>
		</main>
	);
}
```

`ProductPage` is composed from smaller pieces. Make a component when a piece of UI has a clear purpose, is repeated, or is getting hard to understand. A component does not need to be made reusable before there is a reason.

## 4. Props: Give a Component Its Inputs

Props are values passed from a parent component to a child component. They are similar to method parameters: the parent supplies the values and the child uses them. A child should not change its props.

```tsx
type ProductCardProps = {
	name: string;
	price: number;
};

function ProductCard({ name, price }: ProductCardProps) {
	return (
		<article>
			<h2>{name}</h2>
			<p>${price.toFixed(2)}</p>
		</article>
	);
}

function ProductPage() {
	return <ProductCard name="Leather Tote" price={79.99} />;
}
```

The type describes which values the component expects. TypeScript can then report mistakes, such as passing text where a number is required.

## 5. State: Remember Values That Change

State is data a component remembers between renders. Use the `useState` hook when a value changes because of user actions or other events. Calling the setter tells React to render the component again with the new value.

```tsx
import { useState } from "react";

function QuantityPicker() {
	const [quantity, setQuantity] = useState(1);

	return (
		<div>
			<p>Quantity: {quantity}</p>
			<button onClick={() => setQuantity(quantity + 1)}>
				Add one
			</button>
		</div>
	);
}
```

Here, `quantity` is the current value and `setQuantity` changes it. Do not change a state variable directly, such as `quantity = quantity + 1`; use its setter instead.

In Spring Boot terms, component state is closer to temporary UI state in a browser than to a database record. It is not automatically saved on the server and may disappear when the page is reloaded.

## 6. Events and Forms: Respond to the User

React event names use camel case, and you pass a function to them. For a form, keep the input value in state when the UI needs to respond to it as the user types.

```tsx
import { useState } from "react";

function ProductSearch() {
	const [searchText, setSearchText] = useState("");

	return (
		<label>
			Search products
			<input
				value={searchText}
				onChange={(event) => setSearchText(event.target.value)}
			/>
		</label>
	);
}
```

`onChange` runs when the text changes. The input's `value` is controlled by React state, so what the user sees stays in sync with `searchText`.

## 7. Conditions and Lists

Use normal JavaScript conditions to decide what to show. Use `map` to turn an array into a list of elements. Each list item needs a stable `key`, usually an ID from the data.

```tsx
type Product = {
	id: number;
	name: string;
};

const products: Product[] = [
	{ id: 1, name: "Leather Tote" },
	{ id: 2, name: "Canvas Pouch" },
];

function ProductList() {
	if (products.length === 0) {
		return <p>No products found.</p>;
	}

	return (
		<ul>
			{products.map((product) => (
				<li key={product.id}>{product.name}</li>
			))}
		</ul>
	);
}
```

The `key` helps React identify which item changed, was added, or was removed. Avoid using the array position as a key if items can be reordered or removed.

## 8. Fetch Data from a Spring Boot API

React and Spring Boot usually communicate over HTTP. Spring Boot exposes an API endpoint, often returning JSON. The React app sends a request and renders the response. The browser does not connect directly to the database.

Example flow:

```text
React page -> GET /api/products -> Spring Boot controller/service -> database
React page <- JSON response    <- Spring Boot controller/service <- database
```

A small example using the browser's built-in `fetch` API:

```tsx
import { useEffect, useState } from "react";

type Product = {
	id: number;
	name: string;
	price: number;
};

function ProductList() {
	const [products, setProducts] = useState<Product[]>([]);
	const [isLoading, setIsLoading] = useState(true);
	const [error, setError] = useState<string | null>(null);

	useEffect(() => {
		async function loadProducts() {
			try {
				const response = await fetch("http://localhost:8080/api/products");
				if (!response.ok) {
					throw new Error("Could not load products");
				}

				const data: Product[] = await response.json();
				setProducts(data);
			} catch {
				setError("Could not load products. Please try again.");
			} finally {
				setIsLoading(false);
			}
		}

		loadProducts();
	}, []);

	if (isLoading) return <p>Loading products...</p>;
	if (error) return <p role="alert">{error}</p>;

	return (
		<ul>
			{products.map((product) => (
				<li key={product.id}>
					{product.name} - ${product.price.toFixed(2)}
				</li>
			))}
		</ul>
	);
}
```

`useEffect` runs work that connects the component to something outside React, such as loading data. The empty dependency array (`[]`) means this effect runs when the component is first displayed. In a real project, put API calls in a service module once they need to be reused, and keep loading, success, and error states visible to the user.

The browser may block requests if the frontend and backend use different origins and Spring Boot has not allowed the frontend origin with CORS settings. This is a backend configuration issue, not a React rendering issue.

## 9. React and Spring Boot: What Goes Where?

| Concern | React frontend | Spring Boot backend |
| --- | --- | --- |
| Runs in | User's browser | Server |
| Main responsibility | Display screens and respond to user actions | Apply trusted business rules and provide APIs |
| Example | Product page, search input, cart count | Product controller, order service, repository |
| Data | Temporary UI state and data received from APIs | Persistent data, authorization, business records |
| Communication | Sends HTTP requests and reads JSON | Handles HTTP requests and returns JSON |
| Validation | Helps the user correct input quickly | Must validate input again before accepting it |

Frontend validation improves usability, but it cannot protect the application by itself. Users can bypass browser code, so Spring Boot must check permissions and validate important data too.

| React idea | Rough Spring Boot comparison | Important difference |
| --- | --- | --- |
| Component | Thymeleaf fragment or view | A React component runs in the browser and updates interactively. |
| Props | Method parameters or view-model data | Props flow from a parent component to a child component. |
| State | Temporary UI/session-like value | React state belongs to the browser component; it is not automatically persisted. |
| API request | Calling a backend endpoint with an HTTP client | The React app is the HTTP client; Spring Boot handles the request. |
| TypeScript type | Java class/type | TypeScript checks frontend code but does not validate incoming API data at runtime. |

These are comparisons to help you orient yourself, not exact equivalents. React and Spring Boot run in different places and have different responsibilities.

## 10. A Practical Learning Order

Learn and practice these topics in order:

1. JSX and components: build a static product page.
2. Props: show different products using one `ProductCard` component.
3. State and events: add a quantity picker and a search box.
4. Conditions and lists: show results, empty states, and messages.
5. Forms: collect input and show simple validation feedback.
6. API requests: load products from a Spring Boot endpoint and handle loading and errors.
7. Routing: let users move between pages such as products and product details.
8. Shared state and testing: learn these when the app needs shared data or stronger confidence.

Build one small feature at a time. For example, first render a hard-coded product list, then add search, then replace the hard-coded data with an API response.

## 11. What to Learn as Your App Grows

You do not need to learn all of these topics before building your first page. Add them when your application needs them:

- **Routing:** Use a routing library, commonly React Router, when the app has multiple pages. A product list and a product details page can have different URLs, such as `/products` and `/products/42`.
- **API handling:** Learn how to send `GET`, `POST`, `PUT`, and `DELETE` requests, and handle loading, success, and error states. Spring Boot provides the endpoints; React calls them and displays the results. Start with `fetch`; use an API client or data-fetching library if the project needs shared configuration, caching, or refetching.
- **Forms and validation:** Learn to collect input, show helpful validation messages, and submit data to an API. Validate on both sides: browser validation helps the user, while Spring Boot validation protects the backend.
- **Authentication:** Learn how sign-in and protected pages work when the app requires accounts. React can show the appropriate screens, but Spring Security must verify identity and permissions for protected API requests. Do not treat hiding a button or page as backend security.
- **Testing:** Learn to test components and user actions, then test complete browser workflows. Start with a small test for an important component or form; add broader tests as the app becomes more important.
- **Shared state:** Learn React Context or a state-management library only when several distant components need the same changing data, such as a cart. Keep simple state local to the component that uses it.

### Practice Project: Product Browser

Build this in small steps, and keep each step working before moving to the next:

1. Display a hard-coded list of products using a `ProductCard` component.
2. Add a search input and a quantity control using state and events.
3. Add a product details page using routing.
4. Load products from a Spring Boot API and show loading, empty, and error states.
5. Add a form to create or update a product, with validation in the UI and backend.
6. If the app requires users, add sign-in and protect API operations through Spring Security.
7. Add tests for the product list, form, and one important end-to-end workflow.

This project gives you practice with common React work without requiring you to learn advanced libraries first.

## Common Beginner Mistakes

- Changing a state variable directly instead of calling its setter.
- Calling a component like a normal function instead of using JSX, for example use `<ProductCard />`.
- Forgetting a stable `key` on elements rendered from a list.
- Using `class` instead of `className` in JSX.
- Expecting React state to save data after a page reload.
- Assuming frontend validation or hidden buttons enforce backend security.
- Forgetting to show loading and error messages while waiting for an API.

## Key Takeaways

- Components are functions that describe pieces of the screen.
- Props pass information into a component; state stores values that change.
- Events connect user actions to state updates.
- JSX uses JavaScript expressions to describe dynamic UI.
- React renders the frontend; Spring Boot remains responsible for trusted backend rules and data.
- React and Spring Boot commonly communicate through HTTP APIs that exchange JSON.
