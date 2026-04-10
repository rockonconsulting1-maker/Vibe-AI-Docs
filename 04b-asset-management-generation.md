# Asset Management & Vibe AI Image Generation

Vibe AI Studio provides robust built-in capabilities for generating, downloading, and managing digital assets (primarily images) directly within the development workflow. This document details the toolset, best practices, and limitations for Senior Developers and Project Managers.

## 1. Vibe AI Image Generation (`generate_image`)

The Vibe AI Studio can generate high-quality, production-ready images on the fly and automatically host them on a built-in cloud storage CDN.

### Model Tiers
We utilize two distinct models optimized for different use cases:
*   **`fast` (Default):** Extremely rapid generation times. Ideal for standard UI elements, cards, avatars, article thumbnails, and background patterns.
*   **`ultra`:** High-fidelity, slower generation. Reserved for critical, large-format imagery such as full-screen Hero backgrounds, highly detailed product mockups, or prominent marketing banners.

### Aspect Ratios
The Vibe AI can enforce specific aspect ratios to ensure images perfectly fit the intended UI containers without awkward cropping:
*   `1:1` - Avatars, grid items, logos.
*   `4:3` / `3:4` - Standard photography, profile cards.
*   `16:9` - Hero sections, video placeholders, social media previews (OG images).
*   `9:16` - Mobile-first layouts, stories.

### Prompting Guidelines & Limitations
*   **No Typography:** Image generators historically fail at rendering coherent text. Prompts must explicitly avoid requesting text, letters, or words. UI text must always be handled via HTML/CSS overlaying the image.
*   **Thematic Background Matching:** To prevent "floating box" artifacts, prompts must specify the background color matching the app's theme (e.g., "on a solid dark slate background" for dark mode apps).
*   **Human Elements:** For testimonials, team pages, or hero sections, prompts are optimized to include realistic human subjects to increase conversion rates and empathy.

---

## 2. External Asset Ingestion (`download_to_repo`)

When users provide external URLs for logos, reference images, or specific brand assets, the Vibe AI does not hotlink them directly. Hotlinking can lead to broken images if the source goes down, and often violates CORS policies.

### Ingestion Workflow
1.  **Fetch:** The Vibe AI uses the `download_to_repo` tool to fetch the asset from the provided URL.
2.  **Upload:** The asset is securely uploaded to the Vibe AI Studio's cloud storage bucket.
3.  **Implementation:** The Vibe AI receives a permanent, CDN-backed `CLOUD_URL` which is then injected directly into the `src` attribute of `<img>` tags or `background-image` CSS properties.

**Important Architectural Note:** Assets generated or downloaded via these tools are *not* saved locally into the `src/assets` or `public` directories. They are hosted externally. Therefore, developers should never use ES6 imports (e.g., `import logo from './logo.png'`) for Vibe AI-managed assets.

---

## 3. SEO & Social Preview Assets

A critical part of the asset generation pipeline is the automatic creation of Open Graph (OG) and Twitter Card preview images.

### The Two-Step SEO Asset Flow
1.  **Generation:** Before writing the `index.html` file, the Vibe AI generates a specific `16:9` image tailored to represent the brand/application for social sharing.
2.  **Injection:** The resulting cloud URL is injected into the `<meta property="og:image">` and `<meta name="twitter:image">` tags.

*Note: SEO preview images are strictly reserved for metadata and are never reused as visible page content (like hero backgrounds).*

---

## 4. UI Implementation Best Practices

When implementing these assets in the React/Tailwind codebase, the Vibe AI enforces strict UI rules:

### Text Readability Over Images
When text is placed over a Vibe AI-generated background image, the Vibe AI automatically applies:
*   **Strong Overlays:** A minimum of `bg-black/60` or a gradient fade (`bg-gradient-to-b from-black/70 via-black/60 to-black/70`).
*   **Text Shadows:** Subtle CSS text shadows (`[text-shadow:_0_1px_3px_rgba(0,0,0,0.3)]`) as a fallback safety net.

### Edge Blending (Masking)
For dashboard mockups, product shots, or floating UI elements, the Vibe AI frequently utilizes CSS `mask-image` with linear gradients. This ensures the generated image dissolves smoothly into the section background, eliminating harsh rectangular edges and creating a polished, premium aesthetic.

```tsx
// Example of Edge Blending implementation
<div className="relative w-full max-w-2xl mx-auto">
  <div className="absolute inset-0 bg-gradient-to-t from-background to-transparent z-10" />
  <img 
    src="https://cloud-storage-url.com/generated-dashboard.png" 
    alt="Dashboard Preview"
    className="w-full h-auto rounded-lg shadow-2xl"
    style={{ maskImage: 'linear-gradient(to bottom, black 60%, transparent 100%)' }}
  />
</div>
```