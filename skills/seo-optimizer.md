---
name: seo-optimizer
description: Add SEO meta tags, structured data, sitemap
category: frontend
tags: ["frontend", "seo", "meta-tags"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# SEO Optimizer

> Add SEO meta tags, structured data, sitemap

You are an SEO specialist implementing on-page optimization. The user wants to add SEO meta tags, structured data (JSON-LD), and generate a sitemap for better search engine visibility.

## What to check first
- Verify your build tool supports static file generation (Next.js, Gatsby, or a custom build script)
- Check if you have a `public/` or `static/` directory for sitemap output
- Confirm your framework allows head tag injection (React Helmet, Next.js Head, or native HTML)

## Steps
1. Install `next-seo` (Next.js) or create a reusable SEO component that injects meta tags into the document head
2. Define essential meta tags: `title`, `description`, `og:image`, `og:url`, `canonical`, `viewport`, and `charset`
3. Add structured data as JSON-LD (application/ld+json script tag) for Organization, Article, or Product schemas
4. Create a sitemap generator script that crawls your routes and outputs `sitemap.xml`
5. Add robots.txt to control crawler access and point to sitemap location
6. Implement Open Graph and Twitter Card tags for social media sharing
7. Add hreflang tags if your site supports multiple languages
8. Configure your build pipeline to generate sitemap at deploy time

## Code
```javascript
// seo-config.js - SEO metadata configuration
export const seoConfig = {
  siteName: 'My Website',
  siteUrl: 'https://mywebsite.com',
  description: 'Your site description',
  socialImage: 'https://mywebsite.com/og-image.jpg',
  twitter: '@yourhandle',
};

// components/SEOHead.jsx - Reusable SEO component
import Head from 'next/head';

export default function SEOHead({ 
  title, 
  description, 
  canonicalUrl, 
  ogImage, 
  article = false 
}) {
  const { siteUrl, siteName, socialImage } = require('../seo-config');
  const fullTitle = `${title} | ${siteName}`;
  const image = ogImage || socialImage;

  return (
    <Head>
      <title>{fullTitle}</title>
      <meta name="description" content={description} />
      <meta name="viewport" content="width=device-width, initial-scale=1" />
      <link rel="canonical" href={canonicalUrl || siteUrl} />
      
      {/* Open Graph */}
      <meta property="og:site_name" content={siteName} />
      <meta property="og:title" content={title} />
      <meta property="og:description" content={description} />
      <meta property="og:image" content={image} />
      <meta property="og:url" content={canonicalUrl} />
      <meta property="og:type" content={article ? 'article' : 'website'} />
```

*Note: this example was truncated in the source. See [the GitHub repo](https://github.com/Samarth0211/claude-skills-hub) for the latest full version.*

## Common Pitfalls

- Treating this skill as a one-shot solution — most workflows need iteration and verification
- Skipping the verification steps — you don't know it worked until you measure
- Applying this skill without understanding the underlying problem — read the related docs first


## When NOT to Use This Skill

- When a simpler manual approach would take less than 10 minutes
- On critical production systems without testing in staging first
- When you don't have permission or authorization to make these changes


## How to Verify It Worked

- Run the verification steps documented above
- Compare the output against your expected baseline
- Check logs for any warnings or errors — silent failures are the worst kind


## Production Considerations

- Test in staging before deploying to production
- Have a rollback plan — every change should be reversible
- Monitor the affected systems for at least 24 hours after the change



---
*From [CLSkills.in](https://clskills.in/browse) — 2,300+ free Claude Code skills*

