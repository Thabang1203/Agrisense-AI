```markdown
# AGRISENSE.AI

**Live:** aqua-agro-wise.lovable.app

https://aqua-agro-wise.lovable.app/

---

## What I Built

AgriSense.AI is a free, mobile-first web app that lets any South African farmer — urban or rural — diagnose problems with crops, livestock, soil, and irrigation by uploading a single photo. Every answer puts water first, because 60% of South Africa's freshwater goes to agriculture and the country is in a deepening water crisis.

The platform combines a TanStack Start frontend, a Lovable Cloud edge function, and Google's Gemini 2.5 Flash multimodal model via the Lovable AI Gateway. I also built a chat advisor for non-image questions and an Insights page that explains the system design to graders and partners.

## The Problem

South Africa faces a structural water crisis. Agriculture is the largest user, but most farmers have no on-demand access to expert agronomic or water-management advice. The result is wasted water, lost crops, weakened livestock, and rising food prices.

Smallholders in townships have no extension officer to call. Commercial farmers wait days for lab tests and consultant visits. Load-shedding disrupts pumps, cold-chain, and irrigation timing. And existing apps focus only on disease identification — not water, soil, or livestock.

## Who This Is For

**Urban smallholders** in Soweto, Khayelitsha, or Mamelodi can diagnose wilting tomatoes, save water, and get cheap fixes. **Rural commercial farmers** in the Free State, Limpopo, or Eastern Cape can spot livestock condition, schedule irrigation, and cut waste. **Extension officers and NGOs** can triage many farms quickly and share advice. **Students and agri-tech founders** get a reference architecture for AI in South African agriculture.

## The Solution

AgriSense.AI is a four-route web app. The landing page (`/`) explains AI's role in agriculture, water solutions, and inter-sector links. The diagnose page (`/diagnose`) handles photo upload plus six one-tap sample images for instant AI diagnosis. The advisor page (`/advisor`) provides a chat interface for South-Africa-specific agronomic questions. The insights page (`/insights`) shows system design with data-flow diagrams for graders and stakeholders.

Each feature has a water angle. Photo diagnosis identifies crop, livestock, or soil issues from one image and always proposes a water-saving fix first. The AI Advisor chat recommends drip irrigation, mulch, and rainwater harvesting. The new one-tap samples in v1.1 include a leaking pipe and cracked soil. The markdown action plan outputs as Today / This week / Long-term — a three-tier urgency structure for water actions. Everything is mobile-first with camera capture, large tap targets, and low-data design for rural connectivity.

## What's New in v1.1 — One-Tap Sample Diagnosis

Many first-time users — including project graders — don't have a farm photo handy. v1.1 adds six pre-loaded sample images on the diagnose page. Tapping any chip fetches the bundled image, converts it to a File object, and runs the exact same image-diagnosis pipeline as a real upload.

The samples include a sick maize leaf showing interveinal yellowing (nitrogen deficiency or water stress), underweight cattle at body condition score 2/5, wilted vegetables from heat and water stress, a leaking irrigation pipe or valve fault, cracked dry soil showing severe moisture loss, and tomato blight from fungal pathology.

The samples live in `src/assets/deck/`. Each chip is a styled button that calls `onSample(src, filename)`, fetches the bundled asset, wraps the blob in a File, and reuses the existing `onFile` handler. No business logic was duplicated — the diagnosis path is identical to a manual upload.

## How I Position This Against Existing Apps

Plantix does crop disease photo identification with a huge image library, but it has no water focus and no livestock support. Aerobotics offers drone and AI for orchards with high precision, used by South African citrus farmers, but it's enterprise-only and expensive. Khula! is a marketplace connecting South African smallholders to buyers, but it offers no diagnosis or agronomic advice.

AgriSense.AI is the only free, water-first tool that covers crops, livestock, soil, and irrigation in one app, in a South African context.

## Cross-Sector Linkages I Built Into the Advice

Agriculture sits at the centre of the South African economy, so AgriSense actively surfaces these links. Water-wise, 60% of national freshwater is used by agriculture, and smarter irrigation eases urban supply. For transport, cold-chain and rural roads decide whether produce reaches market in good condition. On finance, crop insurance, micro-credit, and Land Bank loans hinge on healthy yields. Energy matters because pumps, cold rooms, and processing plants are hit hardest by load-shedding. Manufacturing depends on agro-processing, packaging, and fertiliser industries relying on raw output. And health connects because food security and nutrition are downstream of a healthy farming sector.

## My Water Solutions Playbook

Every AI response is steered by a system prompt that prioritises water. Common recommendations include drip irrigation plus 5 cm of mulch as the default — this cuts water use by 30 to 60 percent. I recommend rainwater harvesting sized to the user's roof and district rainfall. Greywater reuse on non-edible plant parts comes with safe-handling notes. Pre-dawn watering aligns with load-shedding stages. I include photo-based leak detection on pipes, valves, and float valves. And I suggest drought-tolerant crop swaps like sorghum, cowpea, or amaranth for high-water crops.

## System Design & Architecture

AgriSense.AI is a four-layer system. The client layer uses TanStack Start with React 19, Vite 7, and Tailwind v4, with file-based routes in `src/routes/`. The edge layer is a Supabase Edge Function called `agri-ai` that acts as a thin, CORS-aware proxy with input validation and error mapping. The AI layer uses Google Gemini 2.5 Flash via the Lovable AI Gateway — multimodal, accepting both text and image URLs. The data layer is planned for Supabase Postgres with Storage and Row-Level Security for diagnosis history and farm profiles.

I put the AI call behind an edge function to keep API keys off the client, cap usage, and translate gateway errors like 402 or 429 into farmer-friendly messages.

## Technology Stack

I chose TanStack Start v1 with React 19 for SSR, file-based routing, and type-safe links. Vite 7 gives me a fast dev loop and modern bundling. Tailwind v4 with oklch tokens provides an earth and water palette via CSS variables. I used shadcn-style components for accessible, themeable UI primitives. The backend runs on Lovable Cloud with Supabase for edge functions, authentication, and database readiness. The AI is Lovable AI Gateway pointing to Gemini 2.5 Flash — free tier, multimodal, and fast. For toasts and markdown, I used sonner and react-markdown for friendly UX around AI output.

## Data Flow

Here's what happens when a farmer uses the app. They go to the diagnose page, and `fileToBase64()` converts their image. The app calls `supabase.functions.invoke('agri-ai', { mode: 'image', imageBase64 })`. The edge function builds the prompt and calls the Lovable AI Gateway. Gemini 2.5 Flash returns a markdown plan. ReactMarkdown renders it as Today, This week, and Long-term sections. Then the farmer takes action.

## Installation & Local Setup

This project runs on Lovable Cloud — no separate Supabase account is needed. To run locally, I install with `bun install`. Environment variables are auto-provisioned by Lovable Cloud: `VITE_SUPABASE_URL`, `VITE_SUPABASE_PUBLISHABLE_KEY`, and `LOVABLE_API_KEY`. The dev server runs with `bun run dev`, and I build with `bun run build`.

## Project Structure

The `src/routes/` directory contains `__root.tsx` for the root layout, Toaster, and providers; `index.tsx` for the landing page; `diagnose.tsx` for photo upload and sample chips; `advisor.tsx` for the AI chat; and `insights.tsx` for the system design page. The `src/components/` folder holds `SiteHeader.tsx` and `SiteFooter.tsx`. The `src/lib/` directory has `agri-ai.ts` with `callAgriAI()` and `fileToBase64()`. The `src/assets/` folder contains `hero-farm.jpg` and a `deck/` subfolder with the six sample diagnosis images. The `src/styles.css` file holds Tailwind v4 and oklch tokens. The `supabase/functions/agri-ai/index.ts` file is the edge function proxy.

## Testing & QA

I manually smoke-tested all four routes on an iPhone-class viewport at 502 pixels wide. The sample-chip path was tested end-to-end with all six bundled images. The edge function returns 402 and 429 errors mapped to friendly toasts. Image size is guarded at 8 MB, and non-image MIME types are rejected. Markdown is sanitised through react-markdown with no raw HTML allowed.

## Security & Privacy

The AI API key stays server-side in the edge function — never shipped to the browser. CORS is restricted to standard Lovable hosts on the function. No farmer photos are persisted yet; diagnosis is request-scoped. Future database tables will use Supabase Row-Level Security keyed on `auth.uid()`, with roles living in a dedicated `user_roles` table, not on profiles.

## Roadmap

Right now with v1.1, the web app is live with six sample diagnoses, the AI advisor chat, and a presentation deck. In 30 days, I plan to add isiZulu and Sesotho UI, offline PWA support, and a WhatsApp bot frontend. At 6 months, I want soil-moisture sensor integration, weather alerts, and co-op partnerships. At 12 months, I'm aiming for on-device vision for zero-data farms, plus finance and insurance hooks.

## How I Mapped the Acceptance Criteria

The brief asked me to explain AI's role in agriculture — that lives on the landing page hero and the "How AI helps" sections. I needed to identify three existing apps in the sector, which you'll find on the landing page market section, in slide 10 of my deck, and in section 7 of this document. Linking agriculture to other sectors appears on the landing page sector cards, slide 11, and section 8. Solving a real problem with water is handled by my water-first system prompt, the diagnose page advice, and section 9. System design and implementation lives on the insights page, sections 10 through 12, and slide 12. The style and feel of agriculture comes through in my earth and water oklch palette, Georgia headlines, and hero farm image.

## References

I referenced the Department of Water and Sanitation's National Water Resource Strategy, Stats SA's Census of Commercial Agriculture, public product pages for Plantix, Aerobotics, and Khula!, and the Lovable AI Gateway docs for model catalogue and rate limits, plus TanStack Start docs for file-based routing and SSR.
```
