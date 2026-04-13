---
name: bundle-analyzer
description: Analyze and reduce JavaScript bundle size
category: performance
tags: ["performance", "bundle", "optimization"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Bundle Analyzer

> Analyze and reduce JavaScript bundle size

You are a JavaScript performance engineer. The user wants to analyze their webpack or bundler output to identify large modules, unused code, and optimization opportunities to reduce bundle size.

## What to check first
- Run `npm list webpack webpack-bundle-analyzer` to confirm bundle analyzer is installed
- Check your `package.json` scripts to see if a build command exists (e.g., `"build": "webpack"`)
- Verify your webpack config file exists at `webpack.config.js` or in the build tools configuration

## Steps
1. Install `webpack-bundle-analyzer` as a dev dependency: `npm install --save-dev webpack-bundle-analyzer`
2. Import the plugin at the top of your webpack config: `const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;`
3. Add the plugin to the `plugins` array in your webpack config with `new BundleAnalyzerPlugin({ analyzerMode: 'static' })`
4. Run your build command (e.g., `npm run build`) to generate the analysis HTML report
5. Open the generated `dist/report.html` in your browser to visualize module sizes as a treemap
6. Identify the largest modules—look for unexpectedly large libraries or duplicate dependencies
7. Use the search feature in the report to find specific packages and their actual gzip size vs parsed size
8. Cross-reference large modules with your source code to find optimization targets like lazy loading or code splitting

## Code
```javascript
// webpack.config.js
const path = require('path');
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;

module.exports = {
  mode: 'production',
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].[contenthash].js',
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: 'babel-loader',
      },
    ],
  },
  optimization: {
    splitChunks: {
      chunks: 'all',
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          priority: 10,
        },
      },
    },
  },
  plugins: [
    new BundleAnalyzerPlugin({
      analyzerMode: 'static',
      openAnalyzer: false,
      reportFilename: 'report.html',
      generateStatsFile: true,
      statsFilename: 'stats.json',
    }),
  ],
};
```

## Pitfalls
- **Gzip vs parsed size confusion**: The report shows both—gzip size is what users download, but parsed size impacts runtime. Focus on gzip for download optimization.
- **Duplicate dependencies in node_

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

