---
applyTo: "**/*.{ts,js,svelte}"
---

# Svelte Code Standards

You are able to use the Svelte MCP server, where you have access to comprehensive Svelte 5 and SvelteKit documentation. Here's how to use the available tools effectively:

## Available MCP Tools:

### 1. list-sections

Use this FIRST to discover all available documentation sections. Returns a structured list with titles, use_cases, and paths.
When asked about Svelte or SvelteKit topics, ALWAYS use this tool at the start of the chat to find relevant sections.

### 2. get-documentation

Retrieves full documentation content for specific sections. Accepts single or multiple sections.
After calling the list-sections tool, you MUST analyze the returned documentation sections (especially the use_cases field) and then use the get-documentation tool to fetch ALL documentation sections that are relevant for the user's task.

### 3. svelte-autofixer

Analyzes Svelte code and returns issues and suggestions.
You MUST use this tool whenever writing Svelte code before sending it to the user. Keep calling it until no issues or suggestions are returned.

### 4. playground-link

Generates a Svelte Playground link with the provided code.
After completing the code, ask the user if they want a playground link. Only call this tool after user confirmation and NEVER if code was written to files in their project.

---

# Ultracite Code Standards

This project uses **Ultracite**, a zero-config Biome preset that enforces strict code quality standards through automated formatting and linting.

## Quick Reference

- **Format code**: `bun x ultracite fix`
- **Check for issues**: `bun x ultracite check`
- **Diagnose setup**: `bun x ultracite doctor`

Biome (the underlying engine) provides extremely fast Rust-based linting and formatting. Most issues are automatically fixable.

## Core Principles

Write code that is **accessible, performant, type-safe, and maintainable**. Focus on clarity and explicit intent over brevity.

### Type Safety & Explicitness

- Use explicit types for function parameters and return values when they enhance clarity (in `<script lang="ts">` too)
- Prefer `unknown` over `any` when the type is genuinely unknown
- Use const assertions (`as const`) for immutable values and literal types
- Leverage TypeScript's type narrowing instead of type assertions
- Define prop interfaces strictly using `$props<T>()`
- Use meaningful variable names instead of magic numbers - extract constants with descriptive names

### Modern JavaScript/TypeScript

- Use arrow functions for callbacks and short functions
- Prefer `for...of` loops over `.forEach()` and indexed `for` loops
- Use optional chaining (`?.`) and nullish coalescing (`??`) for safer property access
- Prefer template literals over string concatenation
- Use destructuring for object and array assignments
- Use `const` by default, `let` only when reassignment (or `$state`) is needed, never `var`

### Async & Promises

- Always `await` promises in async functions - don't forget to use the return value
- Use `async/await` syntax instead of promise chains for better readability
- Handle errors appropriately with try-catch blocks or Svelte's `{#await ... :catch}` blocks
- Don't use async functions as Promise executors

### Svelte 5 & Reactivity

- Adopt Runes (`$state`, `$derived`, `$effect`, `$props`) for all reactivity. Avoid legacy `export let` or `$:`
- Use `class` and `for` attributes (not `className` or `htmlFor`)
- Use `$derived` for values that depend on other state. Do not manually update state inside effects to synchronize values
- Use `$effect` sparingly, primarily for side effects (DOM manipulation, analytics). Do not use it for derived logic
- Use `let { propName } = $props();` destructuring pattern
- Use standard HTML attributes (`onclick`, `oninput`) instead of the deprecated `on:` directive
- Use `{#snippet}` for reusable template logic or passing UI slots instead of `slot` or generic props
- Always provide a unique key in `{#each}` blocks: `{#each items as item (item.id)}`
- Use `+page.server.ts` for secure, server-side data fetching
- Use Form Actions for mutations
- Avoid waterfalls in `load` functions, use `Promise.all` where applicable
- Do not mutate props. Treat them as read-only streams
- Use `bind:` directives sparingly, prefer one-way data flow with callback events where clarity is needed
- Use `untrack` inside effects only when you strictly need to read a value without subscribing to it

### Error Handling & Debugging

- Remove `console.log`, `debugger`, and `alert` statements from production code
- Throw `Error` objects with descriptive messages, not strings or other values
- Use `try-catch` blocks meaningfully - don't catch errors just to rethrow them
- Prefer early returns over nested conditionals for error cases

### Code Organization

- Keep `<script>`, `<template>` (implicit), and `<style>` in standard order
- Extract complex business logic into `.ts` or `.svelte.ts` (module context) files
- Keep functions focused and under reasonable cognitive complexity limits
- Extract complex conditions into well-named boolean variables
- Prefer simple boolean checks. Extract complex conditions into `$derived` variables with descriptive names.
- Use early returns to reduce nesting
- Prefer simple conditionals over nested ternary operators
- Group related code together and separate concerns
- Group related imports. Avoid barrel files

### Security

- Add `rel="noopener"` when using `target="_blank"` on links
- Avoid `{@html ...}` unless absolutely necessary. If used, sanitize the input first
- Don't use `eval()` or assign directly to `document.cookie`
- Validate and sanitize user input

### Performance

- Don't wrap static values in `$state`
- Avoid defining inline functions inside `{#each}` loops if possible, reference stable handlers
- Use optimized image handling rather than raw `<img>` tags
- Avoid spread syntax in accumulators within loops
- Use top-level regex literals instead of creating them in loops
- Prefer specific imports over namespace imports
- Avoid barrel files (index files that re-export everything)
- Use dynamic imports `import(...)` for heavy components or libraries

## Testing

- Write assertions inside `it()` or `test()` blocks (Vitest recommended)
- Avoid `done` callbacks in async tests, use async/await instead
- Don't use `.only` or `.skip` in committed code
- Keep test suites reasonably flat - avoid excessive `describe` nesting

## When Biome Can't Help

Biome's linter will catch most issues automatically. Focus your attention on:

1. **Business logic correctness** - Biome can't validate your algorithms
2. **Meaningful naming** - Use descriptive names for functions, variables, and types
3. **Architecture decisions** - Component structure, data flow, and API design
4. **Edge cases** - Handle boundary conditions and error states
5. **User experience** - Accessibility, performance, and usability considerations
6. **Documentation** - Add comments for complex logic, but prefer self-documenting code
7. **Accessibility** - Semantic HTML and ARIA roles are manual choices

---

Most formatting and common issues are automatically fixed by Biome. Run `bun x ultracite fix` before committing to ensure compliance.
