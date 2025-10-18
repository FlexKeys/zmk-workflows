# FlexKeys ZMK Workflows

Custom reusable workflows for building ZMK firmware with self-hosted runner support.

## Overview

This repository contains modified versions of ZMK's official build workflows with added support for self-hosted runners. This allows you to:

- Use your own build infrastructure instead of GitHub-hosted runners
- Save on GitHub Actions minutes
- Run multiple parallel builds with multiple runners
- Maintain full control over the build environment

## Workflows

### `build-user-config-self-hosted.yml`

A modified version of ZMK's `build-user-config.yml` that adds:

- ✅ `runs_on` input parameter to specify custom runners
- ✅ Workspace cleanup before builds
- ✅ Always pulls latest Docker image for consistency
- ✅ Proper user ID mapping for file permissions
- ✅ Special handling for `settings_reset` builds
- ✅ All fixes for self-hosted runner compatibility

## Usage

In your ZMK keyboard configuration repo, replace your `.github/workflows/build.yml` with:

```yaml
name: Build ZMK firmware
on: [push, pull_request, workflow_dispatch]

jobs:
  build:
    uses: FlexKeys/zmk-workflows/.github/workflows/build-user-config-self-hosted.yml@main
    with:
      runs_on: self-hosted  # or "ubuntu-latest" for GitHub-hosted
```

### Inputs

All standard ZMK workflow inputs are supported:

- `build_matrix_path` (default: `"build.yaml"`) - Path to build matrix file
- `config_path` (default: `"config"`) - Path to config directory
- `fallback_binary` (default: `"bin"`) - Fallback binary format
- `archive_name` (default: `"firmware"`) - Archive output name
- `runs_on` (default: `"self-hosted"`) - **NEW!** Runner to use

### Example with GitHub-Hosted Runners

```yaml
jobs:
  build:
    uses: FlexKeys/zmk-workflows/.github/workflows/build-user-config-self-hosted.yml@main
    with:
      runs_on: ubuntu-latest
```

## Differences from Official ZMK Workflow

Based on `zmkfirmware/zmk/.github/workflows/build-user-config.yml@v0.3` with these enhancements:

1. **Flexible Runner Selection**: Add `runs_on` input parameter
2. **Workspace Cleanup**: Prevents permission issues on self-hosted runners
3. **Docker Image Updates**: Always pulls latest image for consistency
4. **User Mapping**: Runs Docker with matching UID/GID
5. **Settings Reset Fix**: Skips extra modules for `settings_reset` shield
6. **yq Installation**: Auto-installs yq on self-hosted runners

## Requirements for Self-Hosted Runners

Your self-hosted runner must have:

- Docker installed and running
- Git 2.43.0 or later
- sudo access for workspace cleanup
- Internet access to pull Docker images

## Maintenance

This workflow is maintained by FlexKeys and may diverge from upstream ZMK as needed for self-hosted compatibility.

To update from upstream ZMK:
1. Check `zmkfirmware/zmk/.github/workflows/build-user-config.yml` for changes
2. Apply changes while preserving self-hosted runner modifications
3. Test with both self-hosted and GitHub-hosted runners

## License

MIT License - Same as ZMK Firmware

## Credits

Based on ZMK Firmware's official build workflow by the ZMK Project.
Modified for self-hosted runner support by FlexKeys.
