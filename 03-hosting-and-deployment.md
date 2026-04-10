# Hosting & Deployment Architecture

This document outlines exactly how I (your Vibe AI assistant) handle the build process, hosting, routing, and performance optimizations for the applications we build together in this studio.

## 1. Build Process & Artifacts

I build your application using **Vite** with the **React SWC** plugin. This provides lightning-fast Hot Module Replacement (HMR) while we work and highly optimized static assets for production.

### What I Can Do:
- **Configure the Build:** I can modify `vite.config.ts` to adjust path aliases, proxy rules, or build targets.
- **Manage Dependencies:** I can install, update, or remove npm packages using my dependency tools, which automatically updates your `package.json`.
- **Generate Artifacts:** When the project is built, it produces a `dist/` directory containing purely static assets: an `index.html` file and an `assets/` folder with minified, chunked, and hashed JavaScript and CSS files.

### Build Commands:
While the platform automatically runs these for our live preview, the standard commands available in the project are:
- `npm run dev`: Starts the Vite development server (this is what powers our live preview).
- `npm run build`: Compiles the TypeScript and bundles the application for production.
- `npm run preview`: Serves the production build locally for testing.

## 2. Hosting & Deployment Environment

### Where and How It Gets Hosted
- **Live Preview:** As we build, the platform automatically hosts a live development server. Every time I write code, the preview updates instantly.
- **Production Hosting:** Because I build standard React Single Page Applications (SPAs), the final output is completely static. The platform handles the underlying serving of these files. 
- **Portability:** You own the code. You can take the exact codebase I generate and host it on any static hosting provider like Vercel, Cloudflare Pages, Netlify, or AWS S3.

## 3. Environment Variables

**Important Limitation:** I **cannot** use, store, or manage environment variables (e.g., `.env` files or `VITE_*` variables). They are not supported in my environment.

### How We Work Around This:
- **Public APIs:** I can connect to any public API directly.
- **Platform Integrations:** For secure data like Calendars or CRM Form Tracking, I use specific tools (`signal_calendar_need` and `signal_form_tracking`) that securely connect to the platform's backend using contextually provided Location IDs and Tracking IDs, entirely bypassing the need for `.env` files.

## 4. Routing & SPA Fallbacks

I build Single Page Applications, meaning the browser only ever loads `index.html`, and React takes over the navigation.

### How I Handle Routing:
- I use `react-router-dom` with a `BrowserRouter` to manage all page transitions seamlessly without page reloads.
- I maintain a centralized routing table in `src/App.tsx`.

### SPA Fallbacks:
- **Client-Side:** I always implement a Catch-All route (`*`) that renders a clean, branded `NotFound` (404) component if a user navigates to a non-existent path.
- **Server-Side:** The underlying hosting infrastructure is automatically configured to rewrite all incoming requests to `index.html`. This ensures that if a user directly visits `https://yoursite.com/dashboard`, the server doesn't throw a 404 error, but instead loads the app and lets React Router render the Dashboard.

## 5. Performance Optimization & Caching Strategies

I automatically implement several strategies to ensure the application is fast and efficient:

### What I Do:
- **Asset Optimization:** I never use raw external image URLs. I download external assets using my `download_to_repo` tool, which uploads them to a fast global CDN and returns an optimized cloud storage URL.
- **Code Splitting:** Vite automatically splits vendor code (like React and Radix UI) from your application code, ensuring browsers can cache libraries efficiently.
- **Lazy Loading:** For larger applications, I can implement `React.lazy()` and `Suspense` to split routes into separate chunks that only load when the user navigates to them.
- **Memoization:** I strategically use `useMemo` and `useCallback` to prevent unnecessary re-renders in complex UI components.
- **Cache-Busting:** The build process automatically appends unique hashes to all compiled JavaScript and CSS files (e.g., `main-a1b2c3d4.js`). This allows the hosting provider to cache these files indefinitely (`Cache-Control: immutable`), while ensuring users instantly get the new version when we deploy an update.
