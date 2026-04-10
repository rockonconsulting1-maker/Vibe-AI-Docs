# Codebase Search & File Reading Capabilities

This document details how the Vibe AI Studio interacts with the local project filesystem to gather context, search for specific code patterns, and read file contents efficiently without overwhelming the context window.

## 1. The Context Window & `<current-code>`

Before the Vibe AI performs any active file reading, it relies on the implicit context provided by the platform. 

### The `<current-code>` Block
Every prompt sent to the Vibe AI includes a `<current-code>` block containing:
- A complete directory structure (`<project-structure>`).
- A list of all available files (`<file-list>`).
- The contents of actively modified or recently accessed files.

**Important Rule for Vibe AI Behavior:**
The Vibe AI is strictly instructed **never** to use file-reading tools for files that are already present in the `<current-code>` block. This prevents redundant token usage and speeds up response times.

## 2. Codebase Search (`search_files`)

When the Vibe AI needs to find where a specific component, function, or variable is used across the project, it utilizes the `search_files` tool. This tool acts as a powerful, regex-enabled `grep` for the codebase.

### Capabilities & Parameters
- **`query` (Regex)**: The search pattern. It supports full regular expressions, allowing the Vibe AI to search for complex patterns (e.g., `export const .* = `).
- **`include_pattern` (Glob)**: Restricts the search to specific directories or file types (e.g., `src/**/*.tsx`).
- **`exclude_pattern` (Glob)**: Excludes specific paths (e.g., `**/*.test.ts`).
- **`case_sensitive` (Boolean)**: Toggles case sensitivity (defaults to false).

### Common Use Cases
1. **Refactoring Impact Analysis**: Finding all imports of a specific component before renaming or modifying its props.
2. **Style Audits**: Searching for hardcoded colors or classes (e.g., `text-white` or `bg-blue-500`) to replace them with design system tokens.
3. **Dependency Checks**: Looking for usages of a specific library across the `src` directory.

## 3. File Reading (`view_file`)

When the Vibe AI identifies a file it needs to understand or modify (and that file is not already in the `<current-code>` block), it uses the `view_file` tool.

### Capabilities & Parameters
- **`file_path`**: The relative path from the project root (e.g., `src/components/ui/button.tsx`).
- **`lines` (Optional)**: Specific line ranges to read (e.g., `"1-50, 100-150"`). 

### Line Range Optimization
By default, `view_file` returns the first 500 lines of a file. For massive files, the Vibe AI is trained to use the `lines` parameter to selectively read the necessary sections. This is critical for maintaining context window efficiency and preventing token overflow.

## 4. Parallel Execution Workflow

The Vibe AI is engineered for maximum efficiency through parallel tool execution. 

When tasked with a complex feature that spans multiple files (e.g., updating a generic layout component that affects three different pages), the Vibe AI will **not** read the files sequentially.

**Example Parallel Workflow:**
```json
// The Vibe AI issues these tool calls simultaneously in a single turn
[
  { "call": "view_file", "args": { "file_path": "src/pages/Home.tsx" } },
  { "call": "view_file", "args": { "file_path": "src/pages/About.tsx" } },
  { "call": "view_file", "args": { "file_path": "src/pages/Contact.tsx" } }
]
```
This batching drastically reduces the time it takes for the Vibe AI to gather the necessary context before it begins writing code.

## 5. Security & Limitations

- **Sandboxed Access**: The Vibe AI can only read files within the current project's root directory. It cannot traverse upwards (e.g., `../../etc/passwd`) or access the host machine's filesystem.
- **Read-Only Context**: The `view_file` and `search_files` tools are strictly read-only. Mutations are handled exclusively by a separate suite of tools (`line_replace`, `write_file`, etc.).