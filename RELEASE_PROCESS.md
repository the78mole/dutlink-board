# Automated Release Process

This repository uses automated semantic versioning and release generation. Every push to the `main` branch will automatically create a new release if there are changes.

## Version Bumping Rules

The version number follows semantic versioning (MAJOR.MINOR.PATCH) and is automatically calculated based on commit messages:

### Patch Version (0.1.0 → 0.1.1)
**Default behavior** for most commits:
- `fix: resolve connection issue`
- `docs: update README`
- `chore: update dependencies`
- `test: add unit tests`

### Minor Version (0.1.0 → 0.2.0)
Commits that add new features:
- `feat: add new USB connector`
- `feature: implement power management`

### Major Version (0.1.0 → 1.0.0)
Breaking changes:
- `feat!: redesign board layout` (exclamation mark indicates breaking change)
- `BREAKING CHANGE: remove legacy connector` (explicit breaking change note)

## Release Assets

Each release automatically includes the following production files:

### Documentation
- **dutlink_schematic.pdf** - Complete schematic documentation
- **dutlink_pcb.pdf** - PCB layout with all layers

### Interactive Tools
- **dutlink_ibom.html** - Interactive HTML Bill of Materials for assembly

### Manufacturing
- **dutlink_manufacturing.zip** - Complete package for PCB fabrication

## Workflow Process

1. **Push to main branch** triggers the workflow
2. **Production files** are generated using KiCad automation
3. **Version is calculated** based on commit messages
4. **Release is created** with calculated version tag
5. **Assets are attached** directly to the release (PDFs and iBOM are not zipped)

## Usage Examples

```bash
# Will create patch release (e.g., v0.1.1)
git commit -m "fix: correct trace width on power lines"

# Will create minor release (e.g., v0.2.0)  
git commit -m "feat: add debug header connector"

# Will create major release (e.g., v1.0.0)
git commit -m "feat!: change connector pinout for compatibility"
```

## Notes

- Releases are only created on pushes to `main` branch
- Pull requests will build production files but won't create releases
- The workflow automatically generates release notes from commit messages
- All production files are recreated for each release to ensure consistency