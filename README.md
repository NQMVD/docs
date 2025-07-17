# Stardive Docs

This documentation site is built with [Docus](https://docus.dev), a documentation-focused theme for Nuxt 3.

## Development

### Prerequisites

- Node.js (v18 or higher)
- npm or yarn

### Local Development

1. Install dependencies:
```bash
npm install
```

2. Start the development server:
```bash
npm run dev
```

3. Open [http://localhost:3000](http://localhost:3000) in your browser.

### Building for Production

```bash
npm run build
```

### Generating Static Site

```bash
npm run generate
```

## Project Structure

```
content/
├── 0.index.md              # Homepage
├── 1.guide/                # Guide section
│   ├── _dir.yml             # Section configuration
│   ├── 0.index.md           # Introduction
│   └── 1.quickstart.md      # Quickstart guide
├── 2.libraries/             # Library documentation
│   ├── rust/                # Rust libraries
│   └── lua/                 # Lua libraries
├── 3.std/                   # Standard library docs
│   ├── rust/                # Rust std docs
│   └── lua/                 # Lua std docs
└── 4.projects/              # Project documentation
```

## Configuration

- `nuxt.config.ts` - Nuxt configuration
- `app.config.ts` - Docus app configuration
- `tokens.config.ts` - Design tokens

## Adding Content

1. Create `.md` files in the `content/` directory
2. Use `_dir.yml` files to configure section titles
3. Number files/directories to control order (e.g., `0.index.md`, `1.guide/`)
4. Use YAML frontmatter for page metadata

## Components

Docus provides many built-in components:

- `::card-group` and `::card` for card layouts
- `::code-group` for tabbed code examples
- `::alert` for notices and warnings
- `::details` for collapsible content

See the [Docus documentation](https://docus.dev) for more details.

## Migration

This project was migrated from Mintlify to Docus. The migration included:

- Converting `docs.json` configuration to Docus format
- Converting `.mdx` files to `.md` format
- Updating component syntax
- Reorganizing content structure
- Migrating assets and styling
