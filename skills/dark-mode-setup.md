---
name: dark-mode-setup
description: Implement dark/light mode toggle
category: frontend
tags: ["frontend", "dark-mode", "theme"]
difficulty: beginner
version: 1.0.0
author: Claude Skills Hub
---

# Dark Mode Setup

> Implement dark/light mode toggle

You are a frontend developer implementing a dark/light mode toggle system. The user wants to create a functional theme switcher that persists user preference and applies styles across the entire application.

## What to check first
- Verify your HTML has a root element (e.g., `<html>`, `<body>`, or `#root` div) where you can apply a `data-theme` or `class` attribute
- Check if you're using CSS custom properties (variables) in your stylesheet — if not, you'll need to define them for both light and dark themes

## Steps
1. Define CSS custom properties (variables) in `:root` for light mode and `[data-theme="dark"]` for dark mode
2. Create a JavaScript function that reads the system preference using `window.matchMedia("(prefers-color-scheme: dark)")`
3. Check localStorage for a saved theme preference with `localStorage.getItem("theme")`
4. Create a toggle function that switches the `data-theme` attribute on the document root element
5. Save the user's choice to localStorage whenever they toggle, using `localStorage.setItem("theme", value)`
6. Initialize the theme on page load by reading localStorage or detecting system preference
7. Add an event listener to the toggle button that calls your theme switch function
8. Optionally, listen for system theme changes with `matchMedia().addEventListener("change", ...)`

## Code
```javascript
// Initialize theme on page load
function initializeTheme() {
  const savedTheme = localStorage.getItem("theme");
  const prefersDark = window.matchMedia("(prefers-color-scheme: dark)").matches;
  const theme = savedTheme || (prefersDark ? "dark" : "light");
  
  document.documentElement.setAttribute("data-theme", theme);
}

// Toggle between light and dark mode
function toggleTheme() {
  const currentTheme = document.documentElement.getAttribute("data-theme");
  const newTheme = currentTheme === "light" ? "dark" : "light";
  
  document.documentElement.setAttribute("data-theme", newTheme);
  localStorage.setItem("theme", newTheme);
}

// Listen for system theme changes
function watchSystemTheme() {
  window.matchMedia("(prefers-color-scheme: dark)").addEventListener("change", (e) => {
    const newTheme = e.matches ? "dark" : "light";
    document.documentElement.setAttribute("data-theme", newTheme);
    localStorage.setItem("theme", newTheme);
  });
}

// Attach toggle button
document.addEventListener("DOMContentLoaded", () => {
  initializeTheme();
  watchSystemTheme();
  
  const toggleButton = document.getElementById("theme-toggle");
  if (toggleButton) {
    toggleButton.addEventListener("click", toggleTheme);
  }
});
```

```css
/* Light mode (default) */
:root {
  --bg-color: #ffffff;
  --text-
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

