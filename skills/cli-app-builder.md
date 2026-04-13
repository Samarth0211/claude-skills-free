---
name: cli-app-builder
description: Build CLI applications with Commander.js
category: cli
tags: ["cli", "commander", "node"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# CLI App Builder

> Build CLI applications with Commander.js

You are a Node.js CLI developer. The user wants to build command-line applications using Commander.js with subcommands, options, arguments, and help text.

## What to check first
- Run `npm list commander` to verify Commander.js is installed (or `npm install commander`)
- Check your Node.js version with `node --version` (Commander.js 11+ requires Node 16+)
- Verify the entry point in `package.json` has a `bin` field pointing to your CLI file

## Steps
1. Create a new file (e.g., `cli.js`) and import Commander with `const { Command } = require('commander')`
2. Create a root program instance with `const program = new Command()` and set `.name()`, `.description()`, and `.version()`
3. Define global options using `.option()` before subcommands — e.g., `.option('-v, --verbose', 'verbose output')`
4. Add subcommands with `.command('name')` chained with `.description()`, `.argument()`, and `.action(callback)`
5. Inside the action callback, access arguments from the first parameter and options from the second parameter (the command object)
6. Use `.argument('<name>')` for required arguments and `.argument('[name]')` for optional ones
7. Add `.option()` to individual commands for command-specific flags like `--output` or `--force`
8. Call `.parse(process.argv)` at the end to trigger argument parsing and execute the matched command

## Code
```javascript
const { Command } = require('commander');
const fs = require('fs');
const path = require('path');

const program = new Command();

program
  .name('mytool')
  .description('A sample CLI application built with Commander.js')
  .version('1.0.0');

program
  .option('-d, --debug', 'enable debug mode')
  .option('-c, --config <path>', 'path to config file');

program
  .command('create <project-name>')
  .description('Create a new project')
  .option('-t, --template <type>', 'project template', 'basic')
  .action((projectName, options, command) => {
    const globalOptions = command.parent.opts();
    console.log(`Creating project: ${projectName}`);
    console.log(`Template: ${options.template}`);
    if (globalOptions.debug) console.log('Debug mode enabled');
    
    const projectDir = path.join(process.cwd(), projectName);
    if (!fs.existsSync(projectDir)) {
      fs.mkdirSync(projectDir, { recursive: true });
      console.log(`✓ Project directory created at ${projectDir}`);
    }
  });

program
  .command('build [entry]')
  .description('Build the project')
  .option('-o, --output <dir>', 'output directory', 'dist')
  .option('--minify', '
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

