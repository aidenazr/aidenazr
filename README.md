# Hi, I'm Aiden

**AI-Assisted Product Developer · Full-stack SaaS, LLM features & Stripe integrations**
Malmö, Sweden · Open to junior roles in AI product development, automation and full-stack web development

For almost a year I have been building software products with Claude Code as my main development tool, taking ideas from an empty folder to deployed web apps and App Store releases with databases, payments and third-party integrations. Most of what I build sits where AI meets an everyday business problem: a support assistant that answers from a company's own content, AI-drafted responses to Stripe disputes, and subscription tools that act directly on a customer's Stripe account. On iOS, I have published apps that analyse chart images with an AI vision model and count push-ups with on-device pose detection.

The products below are live. Their source code is private, and I'm happy to walk through how any of them works.

---

## Featured projects

### ReplyCurrent — AI support assistant for websites
**[replycurrent.com](https://www.replycurrent.com)**

Businesses connect their website, help center and PDFs, and ReplyCurrent turns them into an assistant they can embed on their site to answer visitor questions and collect leads.

- **Problem:** visitors ask questions whose answers already exist somewhere in a site's docs, pricing page or FAQ, but they won't go looking for them.
- **Under the hood:**
  - Retrieval-augmented answers: a crawler worker pulls in site content, which is split into chunks, embedded and searched with pgvector, so replies are grounded in the customer's own pages.
  - Bring-your-own-key support for OpenAI, Anthropic and Google Gemini models, restricted to an allowlist.
  - Backend built as Supabase Edge Functions: chat, crawling, knowledge management, the embeddable widget, Stripe billing and webhooks.
  - Multi-tenant Postgres schema with row-level security on workspace data.
- **Stack:** TypeScript · Next.js · Supabase (Postgres, pgvector, Auth, Edge Functions) · OpenAI / Anthropic / Gemini APIs · Stripe

### DisputeStack — Stripe dispute response builder
**[disputestack.com](https://www.disputestack.com)**

Helps SaaS founders and digital sellers respond to Stripe chargebacks. It guides them through the evidence, builds a customer timeline, drafts a response they review and edit, and exports a clean PDF.

- **Problem:** a founder facing a chargeback often doesn't know which evidence matters or how to structure a convincing response.
- **Under the hood:**
  - AI drafting through the OpenAI API, with separate instructions for each dispute reason.
  - Stripe Connect integration to bring in disputes from a connected account, plus Stripe subscriptions for the three paid plans.
  - Multi-factor authentication, account data export and account deletion.
  - Supabase Postgres schema of around 15 tables, with 29 automated test files.
- **Stack:** TypeScript · Next.js · Supabase · OpenAI API · Stripe (Connect, Billing)

### RetainCue — retention offers for Stripe subscriptions
**[retaincue.com](https://www.retaincue.com)**

A hosted cancellation page for SaaS companies that bill through Stripe. When a customer clicks cancel, RetainCue asks why, shows an offer that matches the reason (a discount, a free billing cycle or a pause) and, if they accept, applies it to their real subscription.

- **Problem:** a plain cancel button ends the subscription without learning why or offering an alternative.
- **Under the hood:**
  - Stripe Connect OAuth; accepted offers are applied through Stripe's subscription and coupon APIs, and every outcome (shown, accepted, applied, failed) is stored as its own state.
  - Signed webhooks plus scheduled reconciliation jobs keep the app's records in line with Stripe.
  - Per-project API keys and a REST endpoint for starting a cancellation session from another app.
  - 22 Postgres migrations with row-level security, SQL tests for the policies, and 170+ Vitest test files.
  - Owner notifications by email through Resend.
- **Built in iterations:** grew out of several earlier versions since May 2026, first released as CancelPause.
- **Stack:** TypeScript · Next.js · Supabase · Stripe Connect · Resend · Zod · Vitest

---

## iOS apps on the App Store

### ChartLens AI — chart analysis and trading journal
**[App Store](https://apps.apple.com/app/id6760035524)**

An iPhone and iPad app for newer traders who want structured feedback on their setups. They photograph or upload a price chart, get an AI analysis they can ask follow-up questions about, and keep a trade journal with AI-written reviews. It is built for learning, not as financial advice.

- **Problem:** newer traders often can't tell why a setup worked or failed, and rarely review their trades consistently.
- **Under the hood:**
  - The app sends the chart image, with the trader's chosen strategy and timeframe, to a Node.js backend on Railway. The backend calls OpenAI's Responses API with image input and parses the structured result before returning it.
  - A follow-up chat about each analysis, plus AI journal reviews (summary, strengths, weaknesses, one improvement tip) returned as JSON.
  - A push-notification service on APNs (HTTP/2 with token authentication), backed by an SQLite scheduler for streaks, inactivity reminders and timezone-aware weekly summaries.
  - Subscriptions through RevenueCat, synced to the backend, and a lessons section whose curriculum is served by the backend.
- **Stack:** Swift · SwiftUI · Node.js · Express · SQLite · OpenAI API (vision) · APNs · RevenueCat · Railway

### PushUp Rivals — push-up game with on-device pose tracking
**[App Store](https://apps.apple.com/app/id6782373587)**

A fitness game where push-ups are the controller. The camera counts each rep, and every rep deals damage to enemies across campaign levels, boss battles and an endless horde mode.

- **Problem:** home workouts get repetitive and are easy to skip.
- **Under the hood:**
  - Real-time body-pose detection with Apple's Vision framework on live camera frames, processed on a background queue that drops late frames to stay responsive.
  - Rep counting from the elbow angle (shoulder, elbow, wrist), smoothed across frames, with a down/up movement state machine, a body-alignment check and a cooldown against double counts.
  - A battle engine that turns reps into damage, plus haptic feedback and RevenueCat subscriptions.
  - 23 XCTest unit tests covering the angle maths, the rep counter and the battle engine. Everything runs on the device; the app has no backend.
- **Stack:** Swift · SwiftUI · Vision · AVFoundation · Core Haptics · RevenueCat · XCTest

### Step N Scroll — screen-time app
**[App Store](https://apps.apple.com/app/id6757087994)**

An iPhone app that keeps distracting apps locked until you have walked enough, with an optional AI "future self" scan.

- **Problem:** it is easy to lose hours to scrolling instead of moving.
- **Under the hood:**
  - Apple's Screen Time APIs (FamilyControls, ManagedSettings) to block and unlock apps, with step data from HealthKit and Core Motion.
  - Subscriptions through RevenueCat, with per-tier scan limits enforced on the server.
  - A TypeScript backend (Fastify, Supabase Postgres and Storage) that runs OpenAI image generation as background jobs.
- **Stack:** Swift · SwiftUI · HealthKit · Screen Time APIs · RevenueCat · Fastify · Supabase · OpenAI API

---

## More projects

- **[Capnivo](https://www.capnivo.io)**: AI captions for short-form video, with Whisper transcription, Gemini-based clip selection and an ffmpeg render worker.
- **[Retriq](https://www.retriq.io)**: failed-payment recovery packaged as a Stripe App, with OAuth, encrypted token storage and automated recovery emails.

---

## Skills

**Most experience with** (used across several deployed products)
- TypeScript, React, Next.js (App Router), Tailwind CSS
- Supabase: Postgres schema design, migrations, row-level security, Auth, Edge Functions
- Stripe: Checkout, Billing Portal, webhooks, Connect
- LLM APIs: OpenAI (including Whisper), Anthropic and Gemini; retrieval with pgvector
- Vitest, Git and GitHub, deploying on Vercel

**Have also used**
- Swift and SwiftUI (Vision, AVFoundation, HealthKit, Screen Time APIs), RevenueCat, APNs push notifications
- Node.js backends with Express and Fastify, SQLite, Resend, ffmpeg
- Flutter and Dart

**AI development tools:** Claude Code (primary), Cursor

---

## How I build

AI writes a large share of my code. My job is deciding what to build, steering the implementation, testing it and understanding it well enough to debug and explain it.

- **Start from the data.** I sketch the database tables and access rules before the screens, and I enforce access with row-level security in the database.
- **Integrate real services.** Stripe, LLM providers and email are part of the product from the start, including webhooks and failure states.
- **Test what can break.** Billing, permissions and data access get automated tests.
- **Review in passes.** I run structured code and design reviews and fix the findings one at a time.
- **Rebuild when it's worth it.** RetainCue is a rebuilt version of my earlier CancelPause product.
- **Ship.** Every featured project is deployed and publicly reachable.

---

## Contact

- Email: [aidenazraya@gmail.com](mailto:aidenazraya@gmail.com)
- GitHub: [github.com/aidenazr](https://github.com/aidenazr)
