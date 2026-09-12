# InkScribe AI — Handwritten Feedback Analysis Dashboard

InkScribe AI is a web application for automatically processing handwritten customer feedback from images and turning it into structured, actionable insights.

Instead of reading a large number of handwritten notes manually, users can upload multiple images and obtain, depending on the selected provider, a transcription and/or an AI-powered analysis including sentiment, themes, rating, confidence, reasoning, and summary.

##  Main Features

*  Handwritten feedback transcription from images
*  AI-powered analysis with multiple providers:

  * Google Gemini (multimodal vision)
  * Groq (vision-capable models)
  * Mistral OCR + Mistral Chat
  * OCR.Space
*  Sentiment classification: `positive`, `neutral`, `negative`
*  Automatic theme/topic extraction
*  Sentiment reasoning and transcription confidence
*  Optional rating extraction from handwritten notes
*  Automatic one-sentence summaries
*  Manual re-analysis of existing feedback
*  AI-generated customer reply drafts
*  Duplicate feedback detection using Jaccard similarity
*  Analytics dashboard with sentiment, rating, theme, word-frequency and time-series visualizations
*  Global filtering and search by sentiment, date, theme and text
*  Custom tags for feedback entries
*  Review workflow for low-confidence results
*  CSV import/export
*  Local persistence with `localStorage`
*  Light/dark display mode
*  Email digest integration with EmailJS
*  Responsive dashboard layout
*  PDF page extraction for image-based feedback processing

##  Architecture

This version is a client-side React application. The browser communicates directly with the selected external API provider.

```text
                         ┌─────────────────────┐
                         │     React / Vite    │
                         │    TypeScript UI    │
                         └──────────┬──────────┘
                                    │
                      Upload / Filter / Analyze
                                    │
                    ┌───────────────┴───────────────┐
                    │       Feedback Store          │
                    │  state + queue + persistence  │
                    └───────────────┬───────────────┘
                                    │
                         Selected AI/OCR provider
                                    │
           ┌────────────────┬───────┴────────┬────────────────┐
           │                │                │                │
      Gemini Vision    Groq Vision      Mistral OCR       OCR.Space
           │                │                │                │
           └────────────────┴───────┬────────┴────────────────┘
                                    │
                           Structured result
                                    │
                 ┌──────────────────┴──────────────────┐
                 │ transcription / sentiment / themes │
                 │ rating / summary / confidence      │
                 └──────────────────┬──────────────────┘
                                    │
                      Dashboard / table / analytics
```

##  Processing Flow

### Gemini / Groq

```text
Image
  ↓
Vision-capable model
  ↓
Transcription + sentiment + themes + rating + summary
  ↓
Structured JSON result
```

### Mistral

```text
Image
  ↓
Mistral OCR
  ↓
Transcription
  ↓
Mistral Chat
  ↓
Sentiment + themes + rating + summary
```

### OCR.Space

```text
Image
  ↓
OCR.Space
  ↓
Transcription
  ↓
Local text analysis
  ↓
Sentiment + themes + rating + summary
```

Using multiple providers also makes it possible to benchmark transcription and analysis quality on different handwriting samples before selecting a preferred solution.

##  Tech Stack

| Technology             | Usage                                   |
| ---------------------- | --------------------------------------- |
| React 19               | UI and component-based architecture     |
| TypeScript 5.6         | Static typing and safer development     |
| Vite 6                 | Development server and production build |
| Material Web           | UI components                           |
| Google Gemini API      | Multimodal image + text analysis        |
| Groq API               | Vision/text inference                   |
| Mistral OCR API        | Handwriting/document OCR                |
| OCR.Space API          | OCR extraction                          |
| EmailJS                | Email digest integration                |
| Browser `localStorage` | Local persistence                       |

##  Getting Started

### Prerequisites

* Node.js 18+ recommended
* npm
* API key for the provider you want to test

### Installation

```bash
git clone <your-repository-url>
cd <your-project-folder>
npm install
```

### Run in development

```bash
npm run dev
```

The Vite development server runs on:

```text
http://localhost:3000
```

### Production build

```bash
npm run build
```

### Preview the production build

```bash
npm run preview
```

##  Provider Configuration

Open the **Settings** page and configure the API credentials for the provider you want to use.

Supported providers:

* **Google Gemini** — multimodal vision analysis
* **Groq** — vision/text analysis through an OpenAI-compatible API
* **Mistral** — OCR followed by text analysis
* **OCR.Space** — OCR extraction followed by local analysis

> **Security note:** this version performs provider calls directly from the browser, so API keys entered in the frontend must be considered exposed to the client. For production, the recommended architecture is a backend/API gateway that stores provider secrets in server-side environment variables and keeps them out of the browser.

##  Analytics

The dashboard provides several views to help interpret the collected feedback:

* Sentiment distribution
* Sentiment evolution over time
* Top recurring themes
* Rating distribution
* Word-frequency visualization
* Global filtering by sentiment and date
* Theme-based filtering

##  Duplicate Detection

The application includes a lightweight duplicate detector based on **Jaccard similarity** between word-token sets.

A similarity threshold of `0.75` is used to identify near-duplicate feedback entries.

```text
Feedback A ──┐
             ├─ token comparison → Jaccard similarity ≥ 0.75 → duplicate
Feedback B ──┘
```

##  Data Persistence

Feedback records and application settings are persisted locally using browser `localStorage`.

For a production deployment, a backend database would be preferable for multi-user access, centralized storage, authentication, backups and scalability.

##  Import / Export

The dashboard supports:

* CSV import
* CSV export
* JSON backup
* PDF page extraction

## AI Output Structure

Each analyzed feedback record can contain:

```text
transcription
sentiment
sentimentReasoning
themes
rating
summary
confidence
needsReview
source
language
tags
duplicateOf
autoReplyDraft
scannedImage
```

The structured result makes the generated analysis easier to display, filter, export and aggregate.

## ⚡ Performance Considerations

The current batch analyzer processes queued images **sequentially** and updates each item's progress/status independently.

For large workloads, possible improvements include:

* limited-concurrency processing
* retry strategies with exponential backoff
* request cancellation
* server-side job queues
* background processing
* API rate-limit awareness

##  Project Objective

The goal of InkScribe AI is to transform unstructured handwritten feedback into structured insights that can be reviewed quickly and used for decision-making.

Instead of manually reading every note, users can quickly identify what customers liked, what problems are recurring, which themes appear most often, and which feedback needs human review.

##  Project Context

This project was developed as part of a challenge focused on exploring different OCR and AI providers for handwritten feedback analysis.

The multiple-provider design was intentionally used to compare transcription and analysis quality across different approaches and real-world handwriting examples.

##  Status

**Prototype / challenge project**

The application is functional as a client-side prototyp
