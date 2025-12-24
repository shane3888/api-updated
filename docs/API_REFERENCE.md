# AI Visibility Platform - API Reference

**Version:** 1.0.0  
**Last Updated:** December 24, 2025  
**Base URL:** `https://your-replit-url.replit.dev` or `https://your-domain.com`

---

## Table of Contents

1. [Overview](#overview)
2. [Authentication](#authentication)
3. [Health & Status](#health--status)
4. [Analysis Endpoints](#analysis-endpoints)
5. [AI Content Generation](#ai-content-generation)
6. [Persona System](#persona-system)
7. [Staging & Preview](#staging--preview)
8. [Sources & Library](#sources--library)
9. [Error Handling](#error-handling)

---

## Overview

The AI Visibility Platform provides AI-powered SEO analysis, content generation, and persona-based optimization for websites. The API uses JSON for all request/response bodies.

### Content Types
- All requests should include `Content-Type: application/json`
- All responses return `application/json`

### Rate Limits
- Standard: 60 requests/minute
- AI endpoints: 10 requests/minute (due to LLM processing time)

---

## Authentication

Currently, the API is open for development. For production, implement API key authentication via headers:

```
X-API-Key: your-api-key
```

---

## Health & Status

### GET /health
Check if the API is running.

**Response:**
```json
{
  "ok": true,
  "status": "AI Visibility API Ready",
  "port": 3000
}
```

### GET /api/settings/status
Get current AI provider configuration status.

**Response:**
```json
{
  "hasOpenRouterKey": true,
  "hasOpenAIKey": true,
  "model": "anthropic/claude-3.5-sonnet",
  "mockMode": false
}
```

---

## Analysis Endpoints

### POST /api/analyze
Analyze a website for AI visibility scoring.

**Request:**
```json
{
  "url": "https://example.com"
}
```

**Response:**
```json
{
  "success": true,
  "url": "https://example.com",
  "totalScore": 72,
  "categories": {
    "PC": {
      "score": 65,
      "explanation": "Good persona coverage for primary audience..."
    },
    "SQ": {
      "score": 70,
      "explanation": "Decent snippet quotability with clear facts..."
    },
    "BP": {
      "score": 75,
      "explanation": "Well-structured content for AI parsing..."
    },
    "SI": {
      "score": 80,
      "explanation": "Strong technical SEO implementation..."
    },
    "DM": {
      "score": 68,
      "explanation": "Domain authority signals present..."
    },
    "FF": {
      "score": 74,
      "explanation": "Fresh content with recent updates..."
    }
  },
  "stagingId": "abc123",
  "stagingUrl": "/stage/abc123?url=https%3A%2F%2Fexample.com"
}
```

**Scoring Categories:**
- **PC** (Persona Coverage): How well content serves different user types
- **SQ** (Snippet Quotability): Likelihood of being cited by AI
- **BP** (Blueprint Parseability): Structure for AI comprehension
- **SI** (SEO Implementation): Technical SEO elements
- **DM** (Domain Authority): Trust and credibility signals
- **FF** (Freshness Factor): Content recency and updates

### POST /api/score
Get quick scoring without full analysis.

**Request:**
```json
{
  "url": "https://example.com"
}
```

**Response:**
```json
{
  "success": true,
  "totalScore": 72,
  "categories": { ... }
}
```

---

## AI Content Generation

### POST /api/ai/content-suggestions
Generate quick SEO content suggestions for any URL.

**Request:**
```json
{
  "pageUrl": "https://example.com/product"
}
```

**Response:**
```json
{
  "success": true,
  "pageUrl": "https://example.com/product",
  "suggestions": {
    "newTitle": "Product Name - Best Solution for X | Brand",
    "newMeta": "Discover our premium product with features A, B, C. Free shipping on orders over $50. Shop now.",
    "sectionIdeas": [
      "Add a comparison table with competitors",
      "Include customer testimonials section",
      "Add FAQ schema markup"
    ],
    "faq": [
      {
        "question": "What makes this product different?",
        "answer": "Our product stands out because of X, Y, and Z features..."
      },
      {
        "question": "How long does shipping take?",
        "answer": "Standard shipping takes 3-5 business days..."
      }
    ],
    "copyBlocks": [
      {
        "type": "hero",
        "content": "Transform your workflow with our innovative solution..."
      }
    ]
  },
  "provider": "openrouter"
}
```

### POST /api/ai/content-generator
Generate complete, production-ready HTML content blocks.

**Request:**
```json
{
  "target_url": "https://example.com/page",
  "target_topic": "Product overview",
  "consumer_profile": "General Consumers",
  "brand_entity": {
    "name": "BrandName",
    "canonical_name": "brandname",
    "brand_type": "e-commerce",
    "testing_claims": ["quality tested", "certified"]
  },
  "constraints": {
    "legal_disclaimer": "Results may vary."
  }
}
```

**Response:**
```json
{
  "success": true,
  "summary": "This content covers the main features and benefits...",
  "recommended_h1": "Complete Guide to Product Name",
  "meta_title": "Product Name - Feature Overview | Brand",
  "meta_description": "Discover everything about Product Name including features, benefits, and pricing. Expert guide with FAQs.",
  "content_html": "<h1>Complete Guide to Product Name</h1><p>...</p><h2>Key Features</h2>...",
  "meta_tags": {
    "title": "Product Name - Feature Overview | Brand",
    "description": "Discover everything about...",
    "canonical": "https://example.com/page",
    "og_title": "Product Name - Feature Overview",
    "og_description": "Discover everything about...",
    "og_type": "article",
    "og_image": "https://example.com/image.jpg",
    "twitter_card": "summary_large_image",
    "twitter_title": "Product Name - Feature Overview",
    "twitter_description": "Discover everything about..."
  },
  "schema_markup": {
    "faq_schema": {
      "@context": "https://schema.org",
      "@type": "FAQPage",
      "mainEntity": [...]
    },
    "article_schema": {
      "@context": "https://schema.org",
      "@type": "Article",
      ...
    },
    "breadcrumb_schema": {
      "@context": "https://schema.org",
      "@type": "BreadcrumbList",
      ...
    }
  },
  "suggested_internal_links": [
    { "anchor": "related product", "suggestedUrl": "/products/related" }
  ],
  "provider": "openrouter"
}
```

### POST /api/ai/expand-section
Expand a specific content section with AI.

**Request:**
```json
{
  "section_type": "faq",
  "topic": "Product benefits",
  "context": "E-commerce health supplements",
  "count": 5
}
```

**Response:**
```json
{
  "success": true,
  "expanded_content": [
    {
      "question": "What are the main benefits?",
      "answer": "The main benefits include..."
    }
  ]
}
```

---

## Persona System

### POST /api/site-profile
Fetch and analyze a site's content profile.

**Request:**
```json
{
  "url": "https://example.com"
}
```

**Response:**
```json
{
  "success": true,
  "profile": {
    "title": "Example Site - Homepage",
    "description": "We provide solutions for...",
    "headings": ["Welcome", "Our Services", "About Us"],
    "textSnippet": "Example Site is a leading provider...",
    "domain": "example.com",
    "url": "https://example.com"
  }
}
```

### POST /api/persona-detect
Detect relevant user personas for a website.

**Request:**
```json
{
  "url": "https://example.com",
  "enableLLM": true
}
```

**Response:**
```json
{
  "success": true,
  "personas": [
    {
      "key": "researcher",
      "name": "Research Verifier",
      "description": "Looking for data, studies, and citations",
      "icon": "📊",
      "confidence": 0.85,
      "intents": ["evidence", "comparisons", "studies"]
    },
    {
      "key": "founder",
      "name": "Founder/SMB",
      "description": "Practical guidance and quick wins",
      "icon": "🚀",
      "confidence": 0.75,
      "intents": ["pricing", "setup", "value"]
    }
  ],
  "inferredVertical": "saas"
}
```

### POST /api/persona-simulate
Simulate persona-based search queries and answers.

**Request:**
```json
{
  "url": "https://example.com",
  "personaKey": "researcher",
  "personaName": "Research Verifier",
  "personaDescription": "Looking for data and citations",
  "intents": ["evidence", "comparisons"],
  "inferredVertical": "saas"
}
```

**Response:**
```json
{
  "success": true,
  "simulation": {
    "prompts": [
      {
        "cluster": "evidence",
        "variants": [
          {
            "personaStyle": "direct",
            "prompt": "What research supports Example product?"
          }
        ]
      }
    ],
    "answers": [
      {
        "prompt": "What research supports...",
        "answer": "According to studies...",
        "suggestedDomains": ["pubmed.gov", "scholar.google.com"],
        "confidence": 0.8
      }
    ],
    "recommendedSources": [
      {
        "domain": "pubmed.gov",
        "weight": 0.25,
        "reasons": ["Academic credibility", "Citation source"]
      }
    ]
  }
}
```

### POST /api/ai-personas
Get AI-generated persona analysis.

**Request:**
```json
{
  "url": "https://example.com"
}
```

### POST /api/persona-insights
Get detailed persona insights with scoring.

**Request:**
```json
{
  "url": "https://example.com",
  "personaKey": "researcher"
}
```

---

## Staging & Preview

### POST /api/stage/ensure
Create or retrieve a staging preview for a URL.

**Request:**
```json
{
  "url": "https://example.com"
}
```

**Response:**
```json
{
  "success": true,
  "stagingId": "abc123",
  "stagingUrl": "/stage/abc123?url=https%3A%2F%2Fexample.com"
}
```

### GET /stage/:id
Get the staged HTML content for preview.

**Query Parameters:**
- `url` (required): The original URL being staged

**Response:** HTML content with staging wrapper for iframe embedding

### GET /api/stage/:id/snapshot
Get a snapshot/screenshot of the staged page.

**Response:**
```json
{
  "success": true,
  "snapshot": "base64-encoded-image-data",
  "timestamp": "2025-12-24T10:30:00Z"
}
```

---

## Sources & Library

### GET /api/sources
Get list of authoritative sources in the library.

**Query Parameters:**
- `vertical` (optional): Filter by vertical (e.g., "supplements", "saas")
- `category` (optional): Filter by category

**Response:**
```json
{
  "sources": [
    {
      "id": "src_001",
      "domain": "pubmed.gov",
      "name": "PubMed",
      "category": "academic",
      "trustScore": 95,
      "vertical": "health",
      "enabled": true
    }
  ]
}
```

### POST /api/sources
Add a new source to the library.

**Request:**
```json
{
  "domain": "example-source.com",
  "name": "Example Source",
  "category": "industry",
  "vertical": "saas",
  "trustScore": 80
}
```

### POST /api/sources/assign
Assign sources to a persona with custom weighting.

**Request:**
```json
{
  "personaKey": "researcher",
  "sources": [
    { "sourceId": "src_001", "weight": 0.3 },
    { "sourceId": "src_002", "weight": 0.2 }
  ]
}
```

### GET /api/persona-priors
Get default persona-source weight assignments.

**Response:**
```json
{
  "priors": {
    "researcher": {
      "pubmed.gov": 0.25,
      "scholar.google.com": 0.2
    },
    "founder": {
      "techcrunch.com": 0.15,
      "producthunt.com": 0.1
    }
  }
}
```

### POST /api/verticals/generate
Generate vertical-specific source recommendations.

**Request:**
```json
{
  "vertical": "supplements",
  "url": "https://example.com"
}
```

### POST /api/verticals/apply
Apply a vertical pack to the current source configuration.

**Request:**
```json
{
  "vertical": "supplements",
  "sources": ["src_001", "src_002"]
}
```

---

## Error Handling

All error responses follow this format:

```json
{
  "error": "Error type or message",
  "details": "Additional context or technical details",
  "code": 400
}
```

### Common Error Codes

| Code | Meaning | Description |
|------|---------|-------------|
| 400 | Bad Request | Invalid request body or missing required fields |
| 401 | Unauthorized | Missing or invalid API key |
| 402 | Payment Required | AI provider credits exhausted |
| 404 | Not Found | Resource or endpoint not found |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Server Error | Internal server error |
| 503 | Service Unavailable | AI provider temporarily unavailable |

### AI Provider Fallback

The system automatically falls back from OpenRouter to OpenAI when:
- OpenRouter credits are exhausted (402)
- OpenRouter rate limit hit (429)
- OpenRouter network errors

Check the `provider` field in responses to see which AI was used.

---

## Demo Pages

Interactive demo pages are available for testing:

- **Content Generator Demo:** `/content-generator-demo.html`
- **AI Suggestions Demo:** `/ai-suggestions-demo.html`

---

## Environment Variables

Required for AI functionality:

| Variable | Description | Required |
|----------|-------------|----------|
| `OPENROUTER_API_KEY` | OpenRouter API key for Claude 3.5 Sonnet | Yes* |
| `OPENAI_API_KEY` | OpenAI API key (fallback) | Yes* |
| `OPENROUTER_MODEL` | Model override (default: anthropic/claude-3.5-sonnet) | No |

*At least one AI provider key is required.

---

## Support

For issues or questions, contact the development team or check the project repository.
