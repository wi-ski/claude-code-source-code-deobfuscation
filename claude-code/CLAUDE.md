# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build and Development Commands

**Development:**
- `npm run dev` - Run CLI in development mode with ts-node
- `npm run build` - Compile TypeScript to JavaScript in dist/
- `npm run start` - Run compiled CLI from dist/
- `npm test` - Run Jest tests
- `npm run lint` - Run ESLint on source files
- `npm run clean` - Remove dist/ directory

**Binary:** The CLI entry point is `dist/src/cli.js` when built, or can be run via `npm run dev`.

## Architecture Overview

This is a TypeScript CLI application with a modular, service-oriented architecture:

### Core Application Structure

**Entry Points:**
- `src/cli.ts` - Direct CLI entry point for command-line usage
- `src/index.ts` - Application bootstrapper for programmatic usage with lifecycle management

**Key Architectural Components:**

**Command System (`src/commands/`):**
- Registry-based command system with argument parsing, validation, and help generation
- Commands are registered in `register.ts` and managed by `index.ts`
- Supports categories, aliases, argument types (string, number, boolean, array), and authentication requirements

**Authentication (`src/auth/`):**
- Dual authentication: API Key and OAuth 2.0 with PKCE
- Event-driven AuthManager with automatic token refresh
- Environment variable support: `ANTHROPIC_API_KEY`
- Token storage currently in-memory (designed for future keychain integration)

**Configuration (`src/config/`):**
- Hierarchical config loading with Zod validation
- Sources (priority order): CLI args → env vars → config files → defaults
- Config file locations: `.claude-code.json`, `~/.claude-code/config.json`, etc.
- Supports nested configuration with dot notation (e.g., `api.baseUrl`)

**AI Integration (`src/ai/`):**
- Claude API client with authentication and connection testing
- Singleton pattern with initialization checking

### Available Commands

**Authentication:**
- `login` - API key or OAuth authentication
- `logout` - Clear stored credentials

**AI Assistance:**
- `ask <question>` - Ask Claude about code/programming
- `explain <file>` - Explain code files with detail levels
- `fix <file>` - Fix bugs with optional issue description
- `refactor <file>` - Refactor code (focus: readability, performance, simplicity, maintainability)
- `generate <prompt>` - Generate code with language specification

**System/Utility:**
- `config [key] [value]` - View/edit configuration
- `run <command>` - Execute terminal commands
- `search <term>` - Search codebase (uses ripgrep if available)
- `edit <file>` - Open files in system editor
- `git <operation>` - Git operations wrapper
- `theme [name]` - Change UI theme (dark/light/system)
- `verbosity [level]` - Set logging verbosity

**Session Management:**
- `clear` - Clear terminal display
- `reset` - Reset AI conversation context  
- `history` - View conversation history (placeholder)
- `commands` - List all available commands
- `help <command>` - Get command help

## Configuration

**Environment Variables:**
- `ANTHROPIC_API_KEY` / `CLAUDE_API_KEY` - API authentication
- `CLAUDE_API_URL` - Override API base URL
- `CLAUDE_LOG_LEVEL` - Logging level
- `EDITOR` - Preferred text editor for edit command

**Key Config Sections:**
```typescript
{
  api: { baseUrl, version, timeout, key },
  auth: { autoRefresh, tokenRefreshThreshold, maxRetryAttempts },
  terminal: { theme, useColors, showProgressIndicators },
  codeAnalysis: { indexDepth, excludePatterns, maxFileSize }
}
```

## Development Notes

- TypeScript with strict mode enabled
- ESM modules with `.js` import extensions
- Error handling through structured error types with categories
- Comprehensive logging system with configurable levels
- Telemetry system with privacy controls
- Cross-platform compatibility (Windows, macOS, Linux)

## Testing and Quality

Run tests with `npm test` and linting with `npm run lint` before making changes.