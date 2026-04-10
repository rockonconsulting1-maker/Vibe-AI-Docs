# Code Modification & File System Operations

This document details the specific tools and methodologies the Vibe AI Studio uses to mutate the codebase, manage dependencies, and ensure surgical, collision-free code updates.

## 1. Surgical Code Editing (`line_replace`)

The Vibe AI's primary and preferred method for modifying existing code is the `line_replace` tool. Instead of rewriting entire files (which is slow and prone to overwriting untracked changes), the Vibe AI performs precise line-based search-and-replace operations.

### How It Works
- **Explicit Line Targets**: The Vibe AI targets exact line numbers (e.g., lines 45 to 82).
- **Ellipsis Matching**: For large blocks, the Vibe AI uses an ellipsis pattern. It provides the first few lines, an ellipsis (`...`), and the last few lines. The engine validates these anchors against the actual file content before executing the replace.
- **Parallel Edits**: The Vibe AI can make multiple independent edits to the same file simultaneously by using the original line numbers from its initial view of the file.

*Benefits for Senior Devs*: This guarantees that unrelated code in the file remains completely untouched, preserving custom logic or formatting that the Vibe AI wasn't explicitly asked to modify.

---

## 2. File Writing & Generation (`write_file`)

When creating entirely new components, pages, or configuration files, the Vibe AI uses the `write_file` tool. 

### "Keep Existing Code" Protocol
If `write_file` must be used as a fallback on a large existing file, the Vibe AI is strictly instructed to use `// ... keep existing code` comments. It will only write the specific sections that need to change, minimizing the footprint of the write operation.

### Import Safety
The Vibe AI follows a strict bottom-up creation rule: **Leaf components are created before parent components**. This prevents Vite build crashes caused by importing files that do not exist yet. For example, `Button.tsx` and `Card.tsx` will be generated before `Dashboard.tsx`.

---

## 3. File System Management

The Vibe AI has full control over the project's file tree through dedicated tools:
- **`rename_file`**: Used to safely rename or move files (e.g., refactoring `components/Header.tsx` to `components/layout/Header.tsx`) without losing file history or creating duplicates.
- **`delete_file`**: Used to clean up unused components, deprecated routes, or placeholder assets.

---

## 4. Dependency Management

The Vibe AI can dynamically manage the project's `package.json` to bring in external libraries as needed.

- **`add_dependency`**: Installs valid npm packages. It supports version pinning (e.g., `lodash@latest`) and installing multiple packages in a single command. It also supports a `dev` flag for devDependencies.
- **`remove_dependency`**: Uninstalls packages and cleans up `package.json`.

*Integration Note:* Since the project is Vite-based, the Vibe AI favors modern, ESM-compatible libraries and avoids Node.js native modules (like `fs` or `path`) in the frontend code.

---

## 5. Automated SEO & Metadata Handling

As part of the code modification workflow, the Vibe AI automatically manages SEO metadata without requiring explicit prompts.

- **`index.html` Metadata**: On initial builds, the Vibe AI automatically rewrites `index.html` to include personalized `<title>`, `<meta name="description">`, and Open Graph (`og:title`, `og:image`, `twitter:card`) tags.
- **Dynamic OG Images**: The Vibe AI generates a custom 16:9 social preview image via the `generate_image` tool specifically for the `og:image` meta tag.
- **On-Page SEO**: When generating pages, the Vibe AI automatically applies semantic HTML (`<main>`, `<article>`, `<nav>`), proper heading hierarchy (single `<h1>`), and descriptive `alt` attributes for all images.