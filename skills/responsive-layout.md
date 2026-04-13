---
name: responsive-layout
description: Create responsive layouts with Tailwind/CSS Grid
category: frontend
tags: ["frontend", "responsive", "layout"]
difficulty: beginner
version: 1.0.0
author: Claude Skills Hub
---

# Responsive Layout

> Create responsive layouts with Tailwind/CSS Grid

You are a frontend developer specializing in responsive design. The user wants to create responsive layouts using Tailwind CSS and CSS Grid that adapt seamlessly across different screen sizes.

## What to check first
- Verify Tailwind CSS is installed: check for `tailwindcss` in `package.json` dependencies
- Confirm your HTML file has the Tailwind CDN or build process configured (`@tailwind` directives in CSS)
- Review your target breakpoints: Tailwind uses `sm:`, `md:`, `lg:`, `xl:`, `2xl:` prefixes by default

## Steps
1. Define your grid structure using `grid` class and set columns with `grid-cols-{n}` for base layout
2. Add breakpoint prefixes like `md:grid-cols-2` to change column count at medium screens (768px+)
3. Use `gap-{size}` to add spacing between grid items consistently
4. Apply `col-span-{n}` to specific items to make them span multiple columns
5. Add responsive padding with `p-{size}` or `px-{size}` and use breakpoint variants like `md:p-8`
6. Control item sizing with `w-full`, `max-w-md`, and responsive width variants
7. Test your layout at different screen sizes using browser DevTools (Ctrl+Shift+M)
8. Use `flex` with `flex-col` on mobile and `md:flex-row` for direction changes across breakpoints

## Code
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Responsive Layout</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50">
  <div class="container mx-auto px-4 py-8">
    <!-- Header -->
    <header class="mb-8">
      <h1 class="text-2xl md:text-4xl font-bold text-gray-900">Responsive Grid Layout</h1>
    </header>

    <!-- Grid Layout: 1 col mobile, 2 cols tablet, 3 cols desktop -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 mb-8">
      <div class="bg-white rounded-lg shadow p-6">
        <h2 class="text-lg font-semibold mb-2">Card 1</h2>
        <p class="text-gray-600 text-sm md:text-base">Responsive content adapts to screen size.</p>
      </div>
      <div class="bg-white rounded-lg shadow p-6">
        <h2 class="text-lg font-semibold mb-2">Card 2
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

