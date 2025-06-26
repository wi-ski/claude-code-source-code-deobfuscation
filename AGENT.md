# Claude Code Development Guide

## Build/Test Commands
- `cd claude-code && npm run build` - Compile TypeScript to dist/
- `cd claude-code && npm run lint` - Run ESLint on src/
- `cd claude-code && npm run test` - Run Jest tests (no tests yet)
- `cd claude-code && npm run dev` - Run CLI in development mode with ts-node
- `cd claude-code && npm run clean` - Remove dist/ directory

## Architecture
- **Main project**: Claude Code CLI - AI coding assistant terminal tool
- **Entry point**: `claude-code/src/cli.ts` - handles command parsing and execution
- **Core modules**: `ai/`, `auth/`, `commands/`, `config/`, `errors/`, `execution/`, `fileops/`, `fs/`, `telemetry/`, `terminal/`, `utils/`
- **Error system**: Comprehensive error handling with categories and severity levels in `errors/types.ts`
- **Commands**: Modular command system with registry pattern in `commands/`

## Code Style
- **TypeScript**: Strict mode, NodeNext module resolution, ES2022 target
- **Imports**: ES modules with `.js` extensions for local imports (e.g., `from './utils/logger.js'`)
- **Formatting**: 2-space indentation, semicolons, single quotes
- **Documentation**: JSDoc comments for all public functions and classes
- **File organization**: Barrel exports via index files
- **Error handling**: Use `UserError` class with appropriate categories and levels
- **Naming**: camelCase for functions/variables, PascalCase for classes/enums
