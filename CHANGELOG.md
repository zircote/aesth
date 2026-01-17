# Changelog

All notable changes to the aesth plugin will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- CONTRIBUTING.md - Guidelines for contributing to the plugin
- ARCHITECTURE.md - Technical architecture and design decisions
- Attribution section acknowledging claude-design-engineer inspiration

### Changed
- Hook files preserved as `.bak` for future reference (`hooks/*.bak`)

### Removed
- Validation hook disabled (conflicted with Subcog PostToolUse hooks)
- Use `/aesth:validate` command for manual validation instead

### Planned
- Add design token export functionality
- Component library generation support

## [0.1.0] - 2026-01-17

### Added
- Initial release of aesth plugin
- **Commands**:
  - `/aesth:init` - Initialize skill and load design system from Subcog
  - `/aesth:status` - Display current design system state
  - `/aesth:validate` - Validate code against design rules
  - `/aesth:capture` - Store patterns and decisions to Subcog
  - `/aesth:extract` - Extract patterns from existing code
- **Skill**: Main aesth skill with craft-focused design principles
- **Reference materials**: Design principles quick reference
- **Hooks**: PostToolUse validation hook (disabled by default)
- **Memory structure**: Consolidated 4-5 memories per project approach
- **Domain scoping**: Project-specific patterns + user-level craft principles
- **Design directions**: Six supported personalities
  - Precision & Density
  - Warmth & Approachability
  - Sophistication & Trust
  - Boldness & Clarity
  - Utility & Function
  - Data & Analysis
- **Validation rules**:
  - Spacing grid compliance (4px base)
  - Depth strategy consistency
  - Color palette adherence
  - Typography hierarchy checks
  - Padding symmetry validation

### Architecture
- Memory-first design using Subcog MCP exclusively
- No file-based fallback (deliberate decision)
- Prompt-based validation hooks
- Tag-based memory organization

### Documentation
- README with installation and usage instructions
- CONTRIBUTING guide for contributors
- ARCHITECTURE document with technical details
- CHANGELOG for version history

[Unreleased]: https://github.com/zircote/aesth/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/zircote/aesth/releases/tag/v0.1.0
