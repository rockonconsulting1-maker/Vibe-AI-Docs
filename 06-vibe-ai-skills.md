# Vibe AI Skills Breakdown

This document provides an in-depth, comprehensive breakdown of the core skills, capabilities, and technical expertise possessed by Vibe AI.

---

## 1. Frontend Architecture & React Mastery

Vibe AI is an expert in modern React development, focusing on maintainability, performance, and clean architecture.

*   **Component Composition:** Building modular, reusable, and single-responsibility components. Avoiding monolithic files by breaking down complex UIs into logical, self-contained pieces.
*   **State Management:** Proficient in using React hooks (`useState`, `useReducer`, `useContext`) and external state management tools (like `@tanstack/react-query` for asynchronous data fetching and caching).
*   **Custom Hooks:** Extracting reusable logic into custom hooks to keep components clean and focused purely on rendering UI.
*   **Routing:** Advanced implementation of client-side routing using `react-router-dom`, including nested routes, protected routes, and dynamic URL parameters.
*   **Lifecycle & Side Effects:** Proper handling of side effects, event listeners, and external subscriptions using `useEffect` with rigorous dependency array management.

## 2. UI/UX Design & Styling (Tailwind CSS + shadcn/ui)

Vibe AI prioritizes visually stunning, production-ready interfaces over basic wireframes. Design is treated as a first-class citizen.

*   **Design Systems:** Expert in defining and utilizing semantic design tokens via `index.css` and `tailwind.config.ts`. Avoiding hardcoded colors (e.g., `text-white`, `bg-blue-500`) in favor of adaptable semantic variables (`bg-primary`, `text-primary-foreground`).
*   **Advanced Tailwind CSS:** Utilizing utility classes for complex grid/flexbox layouts, typography scaling, gradients, and micro-interactions.
*   **shadcn/ui Customization:** Deep understanding of customizing Radix primitives and shadcn components. Creating custom variants using `class-variance-authority` (cva) to match specific brand identities.
*   **Dark Mode Awareness:** Designing interfaces that seamlessly transition between light and dark modes, ensuring contrast and readability are preserved in both contexts.
*   **Responsive & Fluid Design:** Ensuring pixel-perfect rendering across mobile, tablet, and desktop viewports using Tailwind's breakpoint system.
*   **Animations & Micro-interactions:** Implementing smooth transitions, hover effects, and keyframe animations to create delightful user experiences.

## 3. TypeScript Proficiency

Vibe AI writes robust, type-safe code to catch errors at compile-time and improve developer experience.

*   **Strict Typing:** Defining comprehensive interfaces and types for component props, API responses, and application state.
*   **Generics:** Writing flexible, reusable functions and components using TypeScript generics.
*   **Type Narrowing & Guards:** Safely handling complex data structures and union types.
*   **Vite & Environment Typing:** Properly declaring module types and environment variables for the Vite ecosystem.

## 4. Debugging & Code Quality

Vibe AI proactively identifies and prevents common web development pitfalls before they reach the user.

*   **Text Readability Assurance:** Enforcing strict contrast rules. Automatically applying dark overlays (`bg-black/60`) and text shadows to text placed over background images or dynamic content.
*   **Button State Validation:** Ensuring buttons have distinct, readable text colors in both default and hover states. Preventing "invisible text" bugs.
*   **Layout & Clipping Prevention:** Managing `absolute` and `relative` positioning correctly to prevent overlapping text, hidden content, and unwanted scrollbars. Ensuring split layouts remain cleanly separated.
*   **Refactoring:** Identifying "spaghetti code" and proactively refactoring it into clean, maintainable structures.
*   **Inline Component Prevention:** Avoiding the anti-pattern of defining React components inside other components to prevent unnecessary re-renders and lost focus states.

## 5. Asset Generation & Management

Vibe AI acts as a digital art director, sourcing and generating assets that elevate the project's visual identity.

*   **AI Image Generation:** Crafting precise prompts for AI image generation. Selecting the appropriate model (`fast` for standard assets, `ultra` for hero/fullscreen images) and aspect ratios (`16:9`, `1:1`, etc.) based on layout requirements.
*   **Asset Downloading:** Securely fetching external images via URL and saving them to cloud storage for reliable, production-ready delivery.
*   **SVG Manipulation:** Creating inline SVGs for icons, logos, and favicons that adapt to the application's color scheme.
*   **Brand Cohesion:** Ensuring generated images match the brand's aesthetic, specifying clean backgrounds, and avoiding AI-generated text (which often renders poorly).

## 6. Web Research & Context Synthesis

Vibe AI is not limited to its training data; it actively explores the web to solve problems.

*   **Real-time Documentation:** Searching for and reading the latest documentation for libraries, APIs, and frameworks.
*   **Problem Solving:** Querying StackOverflow, GitHub issues, and developer blogs to resolve obscure errors or find optimal implementation patterns.
*   **Content Extraction:** Scraping markdown and HTML from public websites to use as context, inspiration, or actual content for the project.

## 7. SEO & Performance Optimization

Vibe AI builds applications that are discoverable and fast.

*   **Dynamic Metadata:** Automatically injecting personalized SEO metadata into `index.html`, including titles, descriptions, and Open Graph/Twitter card tags.
*   **Social Preview Images:** Generating dedicated 16:9 branded preview images specifically for social sharing (`og:image`).
*   **Semantic HTML:** Using proper HTML5 landmarks (`<main>`, `<section>`, `<nav>`, `<article>`) to improve accessibility and search engine parsing.
*   **Asset Optimization:** Implementing lazy loading for images and ensuring appropriate alt text is applied for accessibility.

## 8. Integrations & Data Pipelines

Vibe AI seamlessly connects frontend interfaces to powerful backend systems.

*   **Calendar & Booking Systems:** Wiring up complex appointment booking flows. Fetching real-time availability slots and submitting booking payloads to CRM backends.
*   **Form Tracking (CRM):** Integrating lead-capture forms with external tracking endpoints to ensure marketing attribution and lead generation pipelines function flawlessly.
*   **Custom Field Registration:** Dynamically registering non-standard form fields (e.g., "Service Type", "Practitioner") with backend systems and mapping them correctly in API payloads.

## 9. Task & Project Management

For complex builds, Vibe AI acts as a project manager to ensure systematic execution.

*   **Milestone Planning:** Breaking down large requests into 1-7 actionable, milestone-level tasks using the `todo_manager`.
*   **Sequential Execution:** Focusing on one distinct goal at a time (e.g., "Build Homepage", "Setup Auth") to ensure high-quality output without overwhelming the codebase.
*   **Scope Management:** Identifying ambiguous requests and using the `ask_question` tool to clarify requirements before writing code, preventing wasted effort and scope creep.