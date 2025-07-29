# Release Process for lightfm-mirror

This document describes how to create releases for the `lightfm-mirror` package on PyPI.

## Prerequisites

1. **PyPI Trusted Publishing Setup**: Configure trusted publishing for `lightfm-mirror`
   - Go to https://pypi.org/manage/account/publishing/
   - Add a new trusted publisher for your GitHub repository
   - Use these settings:
     - Repository name: `sabridw/lightfm`
     - Workflow filename: `release.yaml`
     - Environment name: `release`

2. **GitHub Repository Settings**: 
   - Create a `release` environment in your repository settings
   - Go to Settings → Environments → New environment
   - Name it `release` (must match the workflow)

## Creating a Release

### 1. Update Version

Edit `lightfm/version.py` and bump the version:
```python
__version__ = "1.17.1"  # or whatever the next version should be
```

### 2. Update Changelog

Add entries to `changelog.md` describing the changes in this release.

### 3. Commit Changes

```bash
git add lightfm/version.py changelog.md
git commit -m "Bump version to 1.18"
git push origin master
```

### 4. Create and Push Tag

```bash
git tag v1.18
git push origin v1.18
```

### 5. Create GitHub Release

1. Go to https://github.com/sabridw/lightfm
2. Click "Releases" → "Create a new release"
3. Select the tag you just created (v1.18)
4. Title: "Release 1.18"
5. Description: Copy relevant sections from changelog.md
6. Click "Publish release"

This will automatically trigger the release workflow which will:
- Build wheels for Linux, macOS (Intel + ARM), and Windows
- Build source distribution
- Upload everything to PyPI as `lightfm-mirror`

## Benefits

After release, users can install your package with:

```bash
pip install lightfm-mirror
```

This provides:
- Pre-built wheels (no compilation needed)
- Reliable installation without depending on GitHub availability
- Same API as original lightfm (just different package name)

## Troubleshooting

### Build Failures
- Check the Actions tab for detailed logs
- Most common issues are with C compilation or missing dependencies

### PyPI Upload Failures
- Verify trusted publishing is set up correctly for `lightfm-mirror`
- Check that the `release` environment exists in your repository
- Ensure the version number hasn't already been used

### Version Conflicts
- PyPI doesn't allow re-uploading the same version
- If you need to fix a release, increment the version (e.g., 1.18 → 1.18.1)

## Package Distribution

The wheels support:
- **Platforms**: Linux (x86_64), macOS (Intel + Apple Silicon), Windows (x64)
- **Python versions**: 3.8, 3.9, 3.10, 3.11, 3.12
- **Architecture**: 64-bit systems

This eliminates the need for users to compile from source or use git URLs.