# External Tools, Vibe AI Capabilities & Development Workflow

This document details the Vibe AI Studio's integrated toolset, autonomous development workflow, and project management capabilities. It is designed for Senior Developers and Project Managers to understand how the Vibe AI operates, gathers context, manages assets, and executes multi-step engineering tasks.

---

## 1. Vibe AI-Powered Context & Research Tools

The Vibe AI Studio is not limited to its training data; it has real-time access to the web to ensure it uses the most up-to-date documentation, debugging strategies, and external context.

### Web Search (`web_search`)
- **Real-Time Documentation:** Capable of searching specific domains (e.g., `site:github.com`, `site:docs.anthropic.com`) to fetch the latest API specifications, library updates, and framework patterns.
- **Debugging & Issue Resolution:** Searches StackOverflow and GitHub Issues for esoteric bugs or newly discovered framework quirks.
- **Categorized Queries:** Can filter searches by specific categories such as `github`, `news`, `financial report`, or `pdf` for highly targeted context gathering.
- **Date Awareness:** Automatically adjusts queries to account for the current year (e.g., appending "2026" to find the most recent Vite configurations).

### Web Fetching (`fetch_website`)
- **Direct Scraping:** Can ingest raw HTML or Markdown from any public URL provided by the user or found via search.
- **Context Injection:** Useful when a user pastes a link to a design inspiration, a competitor's site, or a specific technical tutorial they want replicated.

---

## 2. Asset Management & Generation

The Vibe AI Studio handles media assets autonomously, ensuring that the generated application is visually complete without requiring the user to manually upload placeholders.

### Image Generation (`generate_image`)
- **Dual-Model Architecture:**
  - **Fast Model:** Default model used for standard UI elements, avatars, and small illustrative graphics. Optimized for speed.
  - **Ultra Model:** High-fidelity model used for critical, large-scale imagery such as hero banners, full-screen backgrounds, and detailed product mockups.
- **Contextual Prompting:** The Vibe AI automatically writes detailed prompts based on the project's brand, ensuring images fit the industry (e.g., "A confident professional using a sleek dashboard, modern office, 16:9").
- **UI Integration Rules:** 
  - Generates images with clean, minimal backgrounds to prevent visual clutter.
  - Applies CSS `mask-image` or gradient fades to blend images seamlessly into the UI.
  - Strictly avoids generating text within images, relying instead on HTML/CSS overlays for typography.

### External Asset Ingestion (`download_to_repo`)
- **Cloud Storage Bridging:** When a user provides an external image URL (e.g., a company logo), the Vibe AI downloads it and uploads it to secure cloud storage.
- **Direct Referencing:** The Vibe AI uses the resulting persistent cloud URL directly in `<img>` tags or CSS `background-image` properties, ensuring external hotlinking restrictions do not break the app.

---

## 3. Advanced Development Workflow

The Vibe AI Studio employs a highly optimized, parallelized execution strategy to build features faster than traditional sequential prompting.

### Parallel Tool Execution
- **Batch Processing:** The Vibe AI evaluates a request and fires multiple independent tool calls simultaneously. For example, it will read three relevant files, search the web for a specific API, and generate a hero image *at the same time*.
- **Efficiency over Sequence:** Sequential tool calls are strictly avoided unless there is a hard dependency (e.g., waiting for a file read before modifying it).

### Code Modification Strategies
- **Line-Replace Tool (Primary):** For existing files, the Vibe AI uses precise, line-based regex matching to replace only the necessary chunks of code. This minimizes token usage, reduces the risk of overwriting unrelated logic, and is significantly faster than rewriting files.
- **Write-File Tool (Secondary/New Files):** Used exclusively for scaffolding new components or when a file requires a complete architectural rewrite.
- **Component Modularity:** The Vibe AI is instructed to avoid monolithic files. It will proactively break down large pages into smaller, focused React components (e.g., creating `HeroSection.tsx`, `Features.tsx`, and `Footer.tsx` before importing them into `Index.tsx`).

### Multi-Step Task Management (`todo_manager`)
- **Milestone Tracking:** For complex requests (e.g., "Build a full e-commerce flow"), the Vibe AI breaks the project down into 1-7 milestone-level tasks.
- **Stateful Execution:** The UI displays the current active task (e.g., "Building Product Grid"). As the Vibe AI completes code changes, it moves to the next task, ensuring it doesn't lose track of the overarching goal.
- **Dynamic Scoping:** New tasks can be injected mid-flight if the Vibe AI discovers missing dependencies or architectural prerequisites.

---

## 4. Interactive Decision Making

The Vibe AI Studio does not guess when requirements are ambiguous; it engages the user through structured UI elements.

### Clarification Engine (`ask_question`)
- **Interactive UI Cards:** Instead of printing a bulleted list of text options, the Vibe AI triggers an interactive UI card.
- **Concrete Options:** If a user says "Add a dashboard", the Vibe AI will present concrete architectural choices (e.g., "Overview Dashboard", "Analytics Dashboard", "Kanban Board").
- **Workflow Pausing:** Execution halts until the user makes a selection, ensuring no wasted compute or code generation on the wrong path.

---

## 5. Project Management & Best Practices

The Vibe AI acts as an autonomous Senior Developer, enforcing best practices without explicit instruction.

### Automated SEO & Metadata
- **Dynamic `index.html`:** On the first build, the Vibe AI personalizes the `<title>`, `<meta name="description">`, and Open Graph tags based on the inferred business purpose.
- **Social Preview Generation:** It automatically generates a 16:9 branded social preview image and injects it into the `og:image` and `twitter:image` tags.
- **On-Page SEO:** Enforces semantic HTML (`<main>`, `<article>`, `<nav>`), single `<h1>` tags, and descriptive `alt` attributes for all images.

### Design System Enforcement
- **Semantic Tokens:** The Vibe AI is strictly forbidden from using hardcoded utility classes like `bg-white` or `text-black`. It exclusively uses Tailwind semantic tokens (`bg-background`, `text-primary-foreground`) defined in `index.css`.
- **Accessibility & Contrast:** The Vibe AI runs internal heuristics to ensure text readability, specifically enforcing dark overlays (`bg-black/60`) and text shadows when placing text over background images.

### Continuous Feature Suggestion (`suggest_features`)
- At the end of every successful code modification cycle, the Vibe AI provides 2-4 actionable, context-aware suggestions for what to build next, helping Project Managers maintain momentum and explore the platform's capabilities.