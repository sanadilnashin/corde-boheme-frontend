# React Frontend Learning Roadmap

## For Spring Boot Developers Building an E-Commerce Application

---

## Goal

Learn modern frontend development and build a complete React application that communicates with a Spring Boot e-commerce backend.

## Recommended Stack

Frontend Framework : React
Language           : TypeScript
Build Tool         : Vite
Styling            : Tailwind CSS
UI Components      : shadcn/ui
State Management   : Redux Toolkit
API Client         : Axios
Server State Cache : TanStack Query
Forms              : React Hook Form + Zod
Authentication     : JWT + Spring Security
Testing            : Jest + React Testing Library + Playwright
Code Quality       : ESLint + Prettier
Package Manager    : pnpm
Deployment         : Nginx + Docker

## Overall Learning Path

### Phase 1 - Frontend Fundamentals

Understand how the browser-based frontend fits with the Spring Boot backend, and what each technology in the project is responsible for.

#### Frontend Stack Compared with Spring Boot

| Frontend technology | What it does | Spring Boot / Java comparison |
| --- | --- | --- |
| React | Builds the interactive user interface that runs in the browser. | Closest to the view layer, such as Spring MVC with Thymeleaf, but React renders in the browser rather than on the server. |
| TypeScript | Adds static types to JavaScript and compiles to JavaScript for the browser. | Java is also statically typed; TypeScript provides similar type checks for frontend code. |
| Vite | Runs the frontend development server and builds optimized production assets. | Maven or Gradle handle dependency and build tasks; Vite focuses on the frontend development and build pipeline. |
| Tailwind CSS | Styles pages using small, composable utility classes. | There is no backend equivalent; it is part of the browser presentation layer, like using CSS or Bootstrap. |
| shadcn/ui | Provides customizable React UI components such as buttons, dialogs, and tables. | Similar in purpose to reusable Thymeleaf fragments or a UI component library, but designed for React. |
| Redux Toolkit | Manages shared client-side state, such as cart and wishlist interactions. | No direct equivalent: Spring services and session state live on the server, while Redux state lives in the browser. |
| Axios | Sends HTTP requests from the browser to backend APIs. | Similar role to Java HTTP clients such as Spring `RestClient` or `WebClient`, but for frontend requests. |
| TanStack Query | Fetches, caches, and refreshes data received from APIs in the browser. | Related to Spring Cache in its goal of reusing data, but it caches API data on the client rather than on the server. |
| React Hook Form + Zod | Manages browser form state and validates values with schemas. | Comparable responsibilities are handled by Spring MVC form binding and Jakarta Bean Validation on the backend. |
| JWT + Spring Security | The frontend sends credentials or tokens; Spring Security authenticates and authorizes protected API requests. | Spring Security is the backend security framework; the React app integrates with its authentication API and token flow. |
| Jest + React Testing Library + Playwright | Tests frontend logic, components, and complete browser workflows. | Jest and React Testing Library are comparable to JUnit-based tests; Playwright covers end-to-end flows, similar to Selenium-based browser testing. |
| ESLint + Prettier | Finds common JavaScript/TypeScript issues and formats frontend code consistently. | Similar roles to Checkstyle and Spotless in a Java project. |
| pnpm | Installs JavaScript dependencies and records exact versions in a lockfile. | Similar to Maven or Gradle dependency management. |
| Nginx + Docker | Docker packages the application; Nginx can serve frontend assets or act as a reverse proxy. | Docker is used for Spring Boot services too; Nginx can route traffic to the frontend and backend containers. |

#### Benefits

- Large ecosystem and community
- Flexible choice of libraries and architecture

#### Outcome

Understand each frontend technology, its closest Spring Boot comparison, and how the browser application communicates with the backend.

---

### Phase 2 - Environment Setup

For the full setup instructions, see [Environment Setup](Environment-setup.md).

#### Outcome

The development environment is ready after completing the setup guide.

---

### Phase 3 - Create the React Project

Create a React and TypeScript project with Vite:

```bash
pnpm create vite corde-boheme-frontend --template react-ts
cd corde-boheme-frontend
pnpm install
pnpm dev
```

Open the local URL printed by Vite, usually <http://localhost:5173>.

#### Concepts

- Vite development server and build tool
- `package.json` and `pnpm-lock.yaml`
- `node_modules`
- `src` and `public` directories
- TypeScript configuration files

#### Outcome

Run and explore a React application locally.

---

### Phase 4 - Learn React and TypeScript Basics

#### Components and JSX

Components are reusable pieces of UI. JSX lets a component describe the UI it returns.

```tsx
function ProductCard() {
  return <article>Laptop</article>;
}
```

#### TypeScript

JavaScript does not check the intended type of a value. TypeScript adds static types that help catch mistakes during development.

```ts
const name = "Aman";
const typedName: string = "Aman";
```

TypeScript is the recommended choice for this application.

#### Other topics

- Props: pass data into components
- State: store values that change, often with `useState`
- Events: respond to user actions
- Conditional rendering: show UI based on state
- Lists: render collections of data

#### Outcome

Build basic screens from typed, reusable React components.

#### Further Reading

- [React Components and JSX](React-learning.md)
- [TypeScript Basics](Typescript-basics.md)

---

### Phase 5 - Project Structure

Use this as a complete structure to grow into. Files marked `optional` are only needed when you add the corresponding tool or deployment workflow.

```text
corde-boheme-frontend/
├── .github/workflows/ci.yml       # Optional: run lint, tests, and build checks in GitHub Actions.
├── public/                         # Static files served unchanged, such as the favicon.
├── src/
│   ├── app/
│   │   ├── providers.tsx           # Composes app-wide providers such as Redux and TanStack Query.
│   │   ├── router.tsx              # Declares the application's URL routes and page elements.
│   │   └── store.ts                 # Optional: configures the Redux Toolkit store when shared client   state is needed.
│   ├── assets/                      # Imported images, fonts, and other bundled assets.
│   ├── components/ui/               # Shared UI primitives, including components added with shadcn/ui.
│   ├── features/
│   │   ├── products/                # Product domain: API calls, components, hooks, pages, and types.
│   │   ├── cart/                    # Cart domain code; add its subfolders as the feature grows.
│   │   └── auth/                    # Login and account domain code; add its subfolders as needed.
│   ├── lib/
│   │   ├── api-client.ts            # Configures the shared Axios client and request defaults.
│   │   └── query-client.ts          # Configures the TanStack Query cache and defaults.
│   ├── styles/index.css             # Global CSS and Tailwind entry point.
│   ├── test/setup.ts                # Optional: configures shared test matchers and browser-like test   setup.
│   ├── utils/                        # Small domain-independent helper functions.
│   ├── main.tsx                     # Browser entry point that mounts the React application.
│   └── vite-env.d.ts                # Provides TypeScript declarations for Vite and import.meta.env.
├── .env.example                     # Documents required environment variable names without secrets.
├── .gitignore                       # Excludes dependencies, build output, local environment files, and test artifacts.
├── components.json                  # shadcn/ui configuration for component generation.
├── eslint.config.js                 # Configures ESLint rules for JavaScript, TypeScript, and React.
├── index.html                       # Vite's HTML entry document and root element for React.
├── jest.config.ts                   # Optional: configures Jest when using the testing stack in Phase 14.
├── package.json                     # Lists dependencies, package manager, and scripts such as dev, build, and test.
├── pnpm-lock.yaml                   # Locks exact dependency versions; generated and updated by pnpm.
├── playwright.config.ts             # Optional: configures Playwright end-to-end tests.
├── prettier.config.js               # Optional: defines consistent code formatting rules.
├── tsconfig.json                    # Root TypeScript project configuration and references.
├── tsconfig.app.json                # TypeScript settings for browser application source code.
├── tsconfig.node.json               # TypeScript settings for Node-based config files such as vite.config.ts.
├── vite.config.ts                   # Configures Vite, the React plugin, and any Vite integrations.
├── Dockerfile                       # Optional: builds and packages the production frontend container.
├── nginx.conf                       # Optional: configures Nginx to serve the built app and support client-side routes.
└── README.md                        # Project setup, available scripts, environment variables, and development notes.
```

`package.json` is the main project manifest. Its scripts typically include `dev` (`vite`), `build` (`tsc -b && vite build`), `preview` (`vite preview`), and `lint` (`eslint .`); add test and formatting scripts when those tools are installed. Commit `pnpm-lock.yaml` so everyone installs the same dependency versions.

Each feature owns its components, API calls, hooks, and types; keep only genuinely reusable UI in `components/` and shared integrations in `lib/`. Add feature folders such as `orders/`, `checkout/`, and `wishlist/` when those parts of the application are implemented, rather than creating empty folders in advance.

For Tailwind CSS v4, use its Vite plugin and the CSS entry point; a separate `tailwind.config` and PostCSS config are not required for the standard setup. If the project chooses Tailwind v3 or custom PostCSS processing, add the corresponding `tailwind.config.*` and `postcss.config.*` files.

#### Outcome

Know what each project file configures, where application code belongs, and which files can wait until their tools are introduced.

---

### Phase 6 - Routing

Use React Router to navigate between screens without reloading the whole application.

Install it:

```bash
pnpm add react-router-dom
```

Plan routes for:

- Home
- Products and product details
- Cart and checkout
- Orders
- Login and profile

#### Outcome

Navigate between the main pages of the application.

---

### Phase 7 - Styling

#### Styling Options

- **Plain CSS:** direct and built into the browser.
- **SCSS:** adds features such as variables and nesting to CSS.
- **Tailwind CSS:** styles components with utility classes and provides a consistent responsive workflow.

Plain CSS example:

```css
.button {
  color: red;
}
```

SCSS example:

```scss
$primary: blue;
```

Tailwind CSS example:

```html
<button class="rounded bg-blue-500 p-2 text-white">Add to cart</button>
```

Tailwind CSS is the recommendation for this project because it supports a consistent utility-based styling workflow and responsive layouts.

#### Topics

- Utility classes
- Flexbox and grid
- Responsive design
- Page layouts

#### Outcome

Create consistent layouts that work across screen sizes.

---

### Phase 8 - UI Components

Component libraries provide reusable interface elements. Common options include:

- **Material UI:** Google Material Design components; often used for admin panels and dashboards.
- **Ant Design:** a component library commonly used for enterprise and business applications.
- **shadcn/ui:** customizable components that fit well with Tailwind-based applications.

Use shadcn/ui for this project. Typical components include buttons, inputs, cards, dialogs, tables, and dropdowns.

#### Outcome

Build a consistent interface from reusable components.

---

### Phase 9 - Backend Integration

The backend exposes Spring Boot REST APIs. Use Axios in the frontend to make HTTP requests.

Install Axios:

```bash
pnpm add axios
```

Example request:

```ts
axios.get("/api/products");
```

#### Topics

- GET, POST, PUT, and DELETE requests
- Error handling
- Axios instances and interceptors

#### Outcome

Read and update backend data from the frontend.

---

### Phase 10 - Data Fetching and Caching

Repeatedly requesting the same data can waste time and resources. TanStack Query manages server-state fetching, caching, and updates so suitable data can be reused.

```text
Without a cache:
Browser -> API -> API -> API

With a cache:
Browser -> API -> cached data -> reuse when appropriate
```

Use TanStack Query for API data. Keep this distinct from Redux Toolkit, which is useful for client-side state such as cart interactions or user interface state.

#### Outcome

Fetch server data consistently and avoid unnecessary repeat requests where cached data is still valid.

---

### Phase 11 - State Management

Choose state management based on where the state belongs:

- **Context API:** suitable for simple state shared across a small application.
- **Redux Toolkit:** structured, predictable management of shared client state; recommended here for areas such as cart and wishlist state.
- **Zustand:** a smaller, simpler alternative that can suit small-to-medium applications.

Install Redux Toolkit and its React bindings:

```bash
pnpm add @reduxjs/toolkit react-redux
```

Use TanStack Query for server data and caching rather than duplicating that responsibility in Redux.

#### Outcome

Keep shared client state predictable and separate from data owned by the backend.

---

### Phase 12 - Forms and Validation

Forms are needed for login, registration, checkout, addresses, and payment details.

- **React Hook Form:** manages form state and submission.
- **Zod:** defines schemas for validating form data.

Use React Hook Form with Zod so form values and validation rules can be handled consistently.

#### Outcome

Build forms with clear validation and error handling.

---

### Phase 13 - Authentication

The backend uses Spring Security and may issue JWT access and refresh tokens. The frontend integrates with the backend's authentication contract.

#### Typical Flow

```text
User signs in
    -> frontend sends credentials to the backend
    -> backend returns the authentication result
    -> frontend includes valid authentication with protected API requests
    -> backend authorizes or rejects each request
```

For a Spring Boot backend, implement the frontend flow to match the backend API rather than introducing an unrelated authentication provider. Decide token storage and refresh behavior together with the backend's security design.

#### Outcome

Sign users in and access protected APIs according to the backend's security rules.

---

### Phase 14 - Testing

Use different test types for different levels of the application:

- **Jest:** unit tests for logic and utilities.
- **React Testing Library:** component tests based on user-visible behavior.
- **Playwright:** end-to-end tests for workflows in a real browser.

Useful e-commerce flows to test include product browsing, cart updates, login, and checkout.

#### Outcome

Catch logic, component, and user-workflow regressions.

---

### Phase 15 - Monitoring and Analytics

- **Sentry:** collect and investigate frontend errors.
- **Google Analytics:** measure product usage and traffic.
- **Microsoft Clarity:** review user interaction and session insights.

Add monitoring and analytics according to the project's privacy and consent requirements.

#### Outcome

Understand production errors and, where appropriate, how users interact with the application.

---

### Phase 16 - Package Management

Use pnpm to install dependencies and run project scripts. npm is also widely used and is installed with Node.js, but pnpm is the recommendation for this project.

#### Outcome

Use one consistent package manager and commit its lockfile with the project.

---

### Phase 17 - Deployment

Possible frontend hosting options include Vercel and Azure Static Web Apps. The backend can be packaged with Docker and deployed using the infrastructure chosen for the project; Kubernetes may be used when the deployment environment requires orchestration.

Nginx can serve the built frontend or act as a web server or reverse proxy, depending on the deployment architecture.

#### Outcome

Build and deploy the frontend alongside the Spring Boot backend.