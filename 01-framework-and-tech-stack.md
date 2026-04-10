# Vibe AI Studio Capabilities: Framework & Tech Stack

This document serves as the foundational reference for Senior Developers and Project Managers regarding the core technologies, architectural patterns, and structural boundaries of projects generated and managed by Vibe AI Studio.

## 1. Core Architecture & Build Pipeline

The application is built as a highly optimized, modern Single Page Application (SPA).

*   **Core Library**: **React 18** (utilizing concurrent rendering features, hooks, and functional components).
*   **Language**: **TypeScript** (Targeting ES2020). Strict typing is enforced to catch runtime errors at compile time, improving maintainability for large-scale applications.
*   **Build Engine**: **Vite**. We use Vite with the `@vitejs/plugin-react-swc` plugin. 
    *   *Why for PMs/Devs*: Vite provides near-instantaneous Hot Module Replacement (HMR) during development, drastically reducing feedback loops. For production, it uses Rollup to provide highly optimized, tree-shaken static assets.

## 2. UI, Styling, and Design System Engine

Vibe AI Studio strictly adheres to a robust, token-based design system. Ad-hoc styling is strictly prohibited to ensure scalability.

*   **CSS Framework**: **Tailwind CSS** (v3.4+). Used for utility-first styling.
*   **Design Tokens & Theming**: 
    *   All colors are defined as HSL variables in `src/index.css` (e.g., `--primary: 222.2 47.4% 11.2%;`).
    *   `tailwind.config.ts` maps these CSS variables to Tailwind utility classes (e.g., `bg-primary`, `text-muted-foreground`).
    *   *Constraint*: Direct color classes (like `bg-blue-500` or `text-white`) are **never** used. Everything routes through semantic tokens to guarantee flawless Light/Dark mode transitions and brand consistency.
*   **Component Architecture**: **shadcn/ui** + **Radix UI**.
    *   We use Radix UI for headless, accessible (a11y compliant) component primitives (managing focus, keyboard navigation, ARIA attributes).
    *   Components are generated into `src/components/ui/` and are fully owned by the project. They are not an npm dependency, allowing infinite customization.
*   **Variant Management**: `class-variance-authority` (CVA) is used to define component variants (e.g., primary, outline, ghost buttons). `clsx` and `tailwind-merge` are used to dynamically resolve class conflicts safely.

## 3. Routing & Navigation

*   **Router**: **React Router DOM** (v6+).
*   **Implementation**: Client-side routing is handled centrally in `src/App.tsx`.
*   **Capabilities**: Supports nested routes, dynamic URL parameters, and programmatic navigation. A catch-all `*` route is implemented for 404 Not Found handling.

## 4. State Management & Data Fetching

*   **Local UI State**: Managed via standard React Hooks (`useState`, `useReducer`, `useContext`).
*   **Server State & Caching**: **TanStack Query (React Query)** (`@tanstack/react-query`).
    *   *Why for PMs/Devs*: React Query handles all asynchronous state, caching, background synchronization, and automatic retries. This eliminates the need for complex Redux boilerplates for API data management.

## 5. Forms & Data Validation

*   **Form State**: **React Hook Form**. Provides performant, uncontrolled form inputs, minimizing unnecessary re-renders during typing.
*   **Validation Engine**: **Zod**. 
    *   We use Zod for schema declaration. It integrates directly with React Hook Form via `@hookform/resolvers` to provide strict, type-safe validation before any form submission is triggered.

## 6. Testing Infrastructure

*   **Test Runner**: **Vitest**. A Vite-native test runner that shares the same configuration as the build pipeline, ensuring test environments perfectly match production.
*   **DOM Testing**: `@testing-library/react` and `@testing-library/jest-dom` for behavioral, user-centric testing of React components.

## 7. Crucial Boundaries & Limitations (Project Manager Focus)

To properly scope projects with Vibe AI Studio, the following constraints must be understood:

1.  **Frontend Exclusivity**: Vibe AI Studio generates **Frontend-only React applications**. It does not write, deploy, or manage backend servers (Node.js, Python, PHP, Ruby, etc.).
2.  **No Direct Database Access**: The app cannot connect directly to databases (PostgreSQL, MongoDB) using connection strings. All data must be accessed via standard HTTP REST or GraphQL APIs.
3.  **Environment Variables**: The use of dynamic `.env` files (e.g., `VITE_API_KEY`) is not supported within the Vibe AI Studio cloud preview environment. API keys and configurations should be handled via user input, backend proxying, or build-time constants if absolutely necessary.
4.  **Static Export**: The output of the project is a collection of static HTML, CSS, and JS files. There is no Server-Side Rendering (SSR) like Next.js or Remix. It is a pure SPA.
