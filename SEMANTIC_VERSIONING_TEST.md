# Testing Semantic Versioning

This file demonstrates the semantic versioning patterns that will be used by the automated release workflow.

## Test Commit Examples

Here are example commit messages and their expected version bumps:

### Patch Releases (0.1.0 → 0.1.1)
```
fix: resolve trace routing issue on power plane
docs: update component placement guidelines  
chore: clean up KiCad project files
test: add verification script for connectors
```

### Minor Releases (0.1.0 → 0.2.0)
```
feat: add LED status indicators
feature: implement USB-C power delivery support
feat: add debug header for JTAG
```

### Major Releases (0.1.0 → 1.0.0)
```
feat!: change main connector from USB-A to USB-C
BREAKING CHANGE: modify pinout for rev 2.0 compatibility
feat!: redesign board layout for improved signal integrity
```

## Workflow Behavior

1. **On Push to Main**: 
   - Builds production files
   - Calculates semantic version
   - Creates GitHub release with assets

2. **On Pull Request**:
   - Builds production files only
   - No version calculation or release

3. **Assets Included**:
   - dutlink_schematic.pdf
   - dutlink_pcb.pdf
   - dutlink_ibom.html
   - dutlink_manufacturing.zip

This ensures every commit to main results in a properly versioned release with production-ready files.