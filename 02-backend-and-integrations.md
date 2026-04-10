# Backend Architecture & External Integrations

This document outlines the backend architecture, integration capabilities, and data layer strategies available within the Vibe AI Studio development environment. It is tailored for Senior Developers and Project Managers to understand the boundaries and capabilities of the platform.

## 1. Architectural Paradigm: Frontend-First / Serverless

Vibe AI Studio generates **frontend-only React applications** (Vite + React + TypeScript). There is no integrated Node.js, Python, or Ruby runtime for executing backend code directly within the generated project. 

### Implications for Development:
* **API-Driven**: All dynamic data, authentication, and heavy processing must be delegated to external APIs, microservices, or Backend-as-a-Service (BaaS) providers (e.g., Supabase, Firebase, AWS Amplify).
* **Direct Browser Calls**: Network requests are made directly from the client browser. You must ensure external APIs are configured with appropriate CORS (Cross-Origin Resource Sharing) policies.
* **Security**: Never hardcode sensitive secrets (like private API keys or database passwords) in the frontend code. Use secure, client-safe keys or proxy requests through a secure external gateway.

## 2. Data Fetching & Server State Management

The project comes pre-configured with **TanStack Query (React Query)** (`@tanstack/react-query`).

* **Caching & Synchronization**: React Query handles caching, background updates, stale-while-revalidate logic, and error handling out of the box.
* **Standard Implementation**: Use standard `fetch` or `axios` (if installed) wrapped in `useQuery` or `useMutation` hooks.
* **Global State**: For pure client-side state (UI toggles, themes), standard React Context or lightweight libraries like `zustand` are recommended over heavy solutions like Redux.

## 3. Platform Integrations: CRM & Scheduling

Vibe AI Studio includes specialized, built-in tooling for connecting UI components directly to the platform's CRM and scheduling backend (LeadConnector APIs). 

### A. Calendar & Booking Integration
When building appointment schedulers or booking forms, the platform provides seamless integration via specific signals and APIs.

* **Availability (Free Slots)**:
  * Endpoint: `GET /calendars/{calendarId}/free-slots`
  * Features: Fetches available time slots using Unix epoch timestamps and IANA timezones. Limited to 31-day query ranges.
* **Booking Submission**:
  * Endpoint: `POST /vibe-ai/booking/submit`
  * Payload includes standard fields (`firstName`, `lastName`, `email`, `phone`, `selectedSlot`, `timezone`) and dynamic `customFields`.
* **Workflow**: The Vibe AI developer signals the platform when a calendar is needed (`signal_calendar_need`). The platform handles the OAuth/Calendar selection UI, and the generated code is subsequently wired to the selected `calendarId` and `locationId`.

### B. Form Tracking & Lead Capture
For contact forms, newsletter signups, or any lead-capture UI, the platform supports direct CRM ingestion.

* **Tracking Endpoint**: 
  * Endpoint: `POST /external-tracking/events`
  * Event Type: `external_form_submission`
* **Implementation Details**:
  * Tracking is implemented as a "fire-and-forget" background request so it does not block the user's UI experience or primary form submission.
  * Payloads include standard CRM fields mapped from the form data, along with `trackingId`, `locationId`, and `sessionId`.
* **Workflow**: The Vibe AI developer signals the platform (`signal_form_tracking`) when a lead-capture form is built, prompting the user to connect their specific CRM sub-account.

## 4. The Custom Fields Engine

A powerful feature of the platform integration is the **Custom Fields Engine**, which allows arbitrary UI selections to be mapped to the CRM backend.

* **Dynamic Registration**: Any form field that is not a standard contact field (e.g., "Service Selected", "Practitioner Name", "Dietary Restrictions") can be dynamically registered.
* **Data Mapping**: Registered fields return a unique `id` and `key` which are injected into the API payloads (`customFields` array for bookings, or direct keys in `formData` for tracking).
* **UI Agnostic**: Custom fields are not limited to text inputs; they can be mapped to complex UI elements like interactive cards, map selections, or drag-and-drop interfaces.

## 5. Third-Party Integrations & BaaS

Since the environment is purely frontend, it pairs perfectly with modern BaaS solutions:

* **Authentication**: Integrate easily with Supabase Auth, Firebase Auth, Auth0, or Clerk. The React Router setup allows for easy implementation of protected routes and auth guards.
* **Databases**: Connect directly to Supabase (PostgreSQL via PostgREST), Firebase Firestore, or any GraphQL/REST endpoint.
* **Payments**: Stripe Elements or similar client-side SDKs can be integrated, provided the secure intent creation is handled by a separate secure backend or serverless function you control.

## 6. WebSockets & Real-Time Data

Real-time capabilities are fully supported via standard browser APIs:
* **Standard WebSockets**: Native `WebSocket` API integration.
* **Managed Services**: Easy integration with Pusher, Socket.io (client), or Supabase Realtime for chat applications, live dashboards, or collaborative tools.

---
*Next up: Hosting & Deployment details.*