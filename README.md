# ESLint Documentation

This document provides a comprehensive guide to setting up and use ESLint in your projects.

It covers ESLint installation, configuration, rules, and best practices.

## What is ESLint?

ESLint is a popular linting tool for JavaScript and TypeScript. It helps developers find and fix problems in their code, ensuring code quality and adherence to best practices.

## Installation

### **Prerequisites**

- Node.js and npm must be installed.
- A JavaScript or TypeScript project.

### Steps to install ESLint

1. Navigate to your project directory:
    
    ```jsx
    cd /path/to/your/project
    ```
    
2. Install ESLint as a development dependency:
    
    ```jsx
    npm install eslint --save-dev
    ```
    

## Initial Setup

### Creating an ESLint Configuration File

1. Run the ESLint initialization command:

```jsx
npx eslint --init
```

1. Answer the prompts to customize ESLint
    - Choose the type of project (e.g. JavaScript or TypeScript)
    - Select a preferred style guide
    - Specify the environment (e.g. Node.js, Browser)
    - Enable or disable TypeScript support
2. This generates an `.eslintrc` file in your project root.

### **Example Configuration (`.eslintrc.json`)**

```jsx
import globals from "globals";
import pluginJs from "@eslint/js";

/** @type {import('eslint').Linter.Config[]} */
export default [
  {
    languageOptions: {
      globals: {
        window: "readonly", // Allow `window`
        document: "readonly", // Allow `document`
        process: "readonly", // Allow `process`
        __dirname: "readonly", // Allow `__dirname`
      },
    },
    rules: {
      "no-console": "warn",
      "no-debugger": "error",
      curly: ["error", "all"],
      eqeqeq: ["error", "always"],
      "no-eval": "error",
      "no-unused-expressions": "warn",
      yoda: ["error", "never"],
      "prefer-const": "warn"
    },
  },
];
```

## Updating Files

### **Adding ESLint to `package.json`**

```json
 "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "lint": "eslint .",
    "lint:fix": "eslint . --fix"
  },
```

### **Ignoring Files with `.eslintignore`**

```
node_modules/
dist/
build/

```

## Configuring Rules

### Enabling and Disabling Rules

Rules can be configured in the `.eslintrc` file:

- **Error** level: Marks violations as errors.
- **Warning** level: Marks violations as warnings.
    
    ### Example:
    
    ```jsx
    rules: {
          "no-console": "warn",
          "no-debugger": "error",
          curly: ["error", "all"],
          eqeqeq: ["error", "always"],
          "no-eval": "error",
          "no-unused-expressions": "warn",
          yoda: ["error", "never"],
          "prefer-const": "warn"
        },
    ```
    
    ### `"no-console": "warn"`
    
    This rule error when `console.*` or similar methods are used.
    
    ```jsx
    // Example (will trigger an error):
    console.log("This is a log message.");
    console.warn("This is a warning.");
    console.error("This is an error.");
    
    // Corrected:
    // Using a logging library like Winston (or any alternative):
    const logger = require("winston"); // Example: Importing a logger library.
    
    logger.info("This is an info message.");
    logger.warn("This is a warning message.");
    logger.error("This is an error message.");
    
    // Or simply remove unnecessary logging:
    function processData() {
      // Do something meaningful without logging to the console.
    }
    ```
    
    ### `"no-debugger": "error"`
    
    This rule prevents the use of `debugger` statements.
    
    ```jsx
    // Example (will trigger an error):
    debugger;
    
    // Corrected:
    console.log("Debugging information"); // Replace with logging or remove.
    
    ```
    
    ### `curly: ["error", "all"]`
    
    This rule enforces using braces for all control statements, even for single-line blocks.
    
    ```jsx
    // Example (will trigger an error):
    if (condition) console.log("Do something");
    
    // Corrected:
    if (condition) {
      console.log("Do something");
    }
    
    ```
    
    ### `eqeqeq: ["error", "always"]`
    
    This rule enforces the use of strict equality (`===`) instead of loose equality (`==`).
    
    ```jsx
    // Example (will trigger an error):
    if (value == 5) {
      console.log("Match!");
    }
    
    // Corrected:
    if (value === 5) {
      console.log("Match!");
    }
    
    ```
    
    ### `"no-eval": "error"`
    
    This rule warns about expressions that do nothing (like standalone function calls or assignments).
    
    ```jsx
    // Example (will trigger a warning):
    condition && doSomething();
    
    // Corrected:
    if (condition) {
      doSomething();
    }
    ```
    
    ### `"indent": ["error", 2]`
    
    Enforces consistent indentation with **2 spaces**.
    
    ```jsx
    // Example (will trigger an error):
    function greet() {
          console.log("Hello!"); // Indented with 6 spaces.
    }
    
    // Corrected:
    function greet() {
      console.log("Hello!"); // Indented with 2 spaces.
    }
    ```
    
    ### `"prefer-const": "warn"`
    
    Enforces consistent indentation with **2 spaces**.
    
    ```jsx
    // Incorrect (Will Cause ESLint Error)
    let name = "John"; // 'name' is never reassigned, so ESLint will throw an error
    console.log(name);
    ```
    
    ```jsx
    // Correct (If Value Doesn't Change)
    const name = "John"; // Use 'const' because 'name' is not changing
    console.log(name);
    ```
    
    ```jsx
    // Correct (If Value Changes)
    let username = "John";
    username = "Doe"; // Now, 'username' is changing, so 'let' is valid
    console.log(username);
    ```
    

## Using ESLint

### Linting Files

To check your code for linting issues:

```
npm run lint
```

### Auto-fixing Issues

To automatically fix issues where possible:

```
npm run lint:fix
```

## Integrations

### VS Code

1. Install the "ESLint" extension from the VS Code marketplace.
2. Enable "Auto Fix On Save" in your VS Code settings:
    
    ```
    "editor.codeActionsOnSave": {
      "source.fixAll.eslint": true
    }
    ```
    

### Prettier Integration

If using Prettier, install and configure `eslint-config-prettier`:

```
npm install eslint-config-prettier --save-dev
```

Add it to your `.eslintrc` file:

```
"extends": [
  "eslint:recommended",
  "prettier"
]
```

## Common Rules

| **Rule Name** | **Description** | **Level** | **Example** |
| --- | --- | --- | --- |
| `semi` | Enforce semicolons | Error | **Incorrect:** `const x = 5`  **Correct:** `const x = 5;` |
| `quotes` | Use single or double quotes consistently | Error | **Incorrect:** `const a = "hello"; const b = 'world';`  **Correct:** `const a = "hello"; const b = "world";` |
| `eqeqeq` | Require the use of `===` and `!==` | Warning | **Incorrect:** `if (a == 5)`  **Correct:** `if (a === 5)` |
| `no-console` | Warns against using `console` methods | Warning | **Incorrect:** `console.log("Debug");`  **Correct:** `// Remove console.log or use a logging library.` |
| `no-debugger` | Disallows `debugger` statements | Error | **Incorrect:** `debugger;`  **Correct:** `// Remove debugger.` |
| `curly` | Enforces curly braces for all control statements | Error | **Incorrect:** `if (a) console.log(a);`  **Correct:** `if (a) { console.log(a); }` |
| `no-eval` | Disallows `eval()` to prevent security risks | Error | **Incorrect:** `eval("2 + 2");`  **Correct:** `const result = 2 + 2;` |
| `no-unused-expressions` | Warns about unused expressions with no side effects | Warning | **Incorrect:** `value && doSomething();`  **Correct:** `if (value) { doSomething(); }` |
| `yoda` | Disallows "Yoda conditions" (e.g., `5 === value`) | Error | **Incorrect:** `if (5 === value)`  **Correct:** `if (value === 5)` |
| `no-var` | Disallows `var`; use `let` or `const` instead | Error | **Incorrect:** `var x = 5;`  **Correct:** `let x = 5;` or `const x = 5;` |
| `indent` | Enforces consistent indentation (e.g., 2 spaces) | Error | **Incorrect:** `function greet() {\n console.log("Hi!");\n}`  **Correct:** `function greet() {\n console.log("Hi!");\n}` |

For the complete list of rules, visit the [ESLint rules documentation](https://eslint.org/docs/rules/).

## Best Practices

- Always lint your code before committing changes.
- Use the `lint:fix` script to automatically resolve minor issues.
- Keep your `.eslintrc` file consistent across all projects.
- Periodically review and update your ESLint configuration to align with modern practices.

  ---

**Company Name:** Endless Raven

**Writer:** Pasith Senevirathna

**Position:** Backend Developer and DevOps

**Contact Email:** [pasith.senevi02@gmail.com](mailto:pasith.senevi02@gmail.com)
