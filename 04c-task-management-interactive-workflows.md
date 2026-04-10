# Task Management & Interactive Workflows

This document outlines the advanced workflow capabilities of the Vibe AI Studio, focusing on how it manages complex, multi-step projects, clarifies ambiguous requirements, and executes tasks in parallel for maximum efficiency.

## 1. Multi-Step Task Management (`todo_manager`)

For complex requests that require multiple distinct steps (e.g., "Build a full e-commerce flow with a product page, cart drawer, and checkout form"), the Vibe AI utilizes an internal `todo_manager` to break down the project into trackable milestones.

### Core Workflow Actions
- **`set_tasks`**: Initializes a project breakdown. The Vibe AI creates 1 to 7 milestone-level tasks. The first task automatically begins in an `in_progress` state, while the rest are `pending`.
- **`move_to_task`**: Upon completing the current milestone, the Vibe AI marks it as complete and transitions focus to the next specified task.
- **`add_task`**: If new requirements are discovered mid-flight (e.g., needing a shared utility component before building a page), the Vibe AI can dynamically append tasks to the pending list.

### Project Management Rules
1. **Milestone-Level Granularity**: Tasks represent significant deliverables (e.g., "Build Homepage", "Setup Auth Context"). The Vibe AI avoids micro-steps like "Update CSS" or vague tasks like "Polish".
2. **One Page = One Task**: A single cohesive page build is kept as one task rather than fragmented.
3. **UI Visibility**: The task list is reflected in the user interface, providing real-time transparency into the Vibe AI's progress and current focus.

---

## 2. Interactive Clarification (`ask_question`)

When a user's request is ambiguous or open to multiple valid technical interpretations, the Vibe AI is strictly instructed **not to guess or make assumptions**. Instead, it pauses the workflow and uses the `ask_question` tool.

### How It Works
Instead of listing options as plain text (which creates a poor UX), the Vibe AI generates an interactive UI card presenting **2-4 concrete implementation options**. 

### Use Cases for Clarification
- **Vague Requests**: "Add a dashboard" -> *Options: Overview Dashboard (metrics), Analytics Dashboard (charts), Kanban Board (drag-and-drop).*
- **Design Variations**: "Create a pricing section" -> *Options: Monthly/Yearly Toggle, Tiered Cards, Feature Matrix.*
- **Feature Scope**: "Add a contact form" -> *Options: Standard Form, Multi-step Lead Gen, Form with Booking Calendar.*

**Developer Note:** The Vibe AI's turn ends immediately after invoking this tool, waiting for the user's explicit selection before writing any code.

---

## 3. Parallel Execution & Efficiency

To maximize speed and minimize latency, the Vibe AI Studio engine supports concurrent tool execution. The Vibe AI is trained to batch operations whenever possible.

### Parallelization Strategies
- **Batch File Reading**: If the Vibe AI needs context from `App.tsx`, `tailwind.config.ts`, and `index.css`, it triggers three concurrent `view_file` calls rather than reading them sequentially.
- **Batch File Writing**: When scaffolding a new feature (e.g., creating a parent component and three child components), the Vibe AI writes all files simultaneously.
- **Mixed Operations**: The Vibe AI can trigger a Vibe AI image generation (`generate_image`), search the codebase (`search_files`), and read a file (`view_file`) all at the exact same time.

*Architectural Impact:* This parallel execution model reduces the time-to-completion for multi-file refactors from minutes to seconds.

---

## 4. Post-Implementation (`suggest_features`)

At the conclusion of a successful code modification session, the Vibe AI analyzes the new state of the project and invokes the `suggest_features` tool.

- **Summary**: Provides a 1-3 sentence technical summary of the changes made.
- **Actionable Next Steps**: Generates 2-4 highly specific, actionable suggestions for what to build next (e.g., "Add empty state to cart drawer", "Implement dark mode toggle").
- **Scope**: Suggestions are always focused on frontend UI/UX, animations, and component additions, keeping the project momentum moving forward.