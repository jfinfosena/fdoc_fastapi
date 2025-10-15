# Testing CodeBlock and TabbedCodeBlock with +++ Syntax

This page tests the new +++ syntax for CodeBlock and TabbedCodeBlock components.

## CodeBlock Test

Here's a CodeBlock using the new +++ syntax:

+++code-block
```javascript
function greet(name) {
    console.log(`Hello, ${name}!`);
    return `Welcome to our application, ${name}`;
}

// Example usage
greet("World");
```
+++

## TabbedCodeBlock Test

Here's a TabbedCodeBlock using the new +++ syntax:

+++tabbed-code-block
```javascript
// File: main.js
function main() {
    console.log("Main application started");
}

---
// File: utils.js
function helper() {
    return "Utility function";
}

---
// File: config.js
const config = {
    apiUrl: "https://api.example.com",
    timeout: 5000
};
```
+++

## Traditional Code Block (for comparison)

Here's a traditional code block for comparison:

```javascript
function traditionalExample() {
    console.log("This is a traditional code block");
}
```

## Summary

Both CodeBlock and TabbedCodeBlock components should now work with the +++ syntax while maintaining backward compatibility with traditional usage.