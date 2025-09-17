# Copilot Instructions for conda-zsh-completion

This project provides Zsh completion for the Conda package manager and related tools (Mamba, Micromamba). Here's what you need to know to be productive in this codebase:

## Project Structure

- `_conda`: The main completion script that follows Zsh completion system conventions
- `conda-zsh-completion.plugin.zsh`: Plugin file for Oh-My-Zsh/other Zsh frameworks
- `.github/`: GitHub-specific files including this guide

## Core Concepts

### Completion System Architecture

The completion system is built around these key components:

1. **Environment Listing**: Handles completion for conda environments using `__conda_envs()`
   - Supports both global environments (in conda base) and local environments (in ~/.conda/envs)
   - Customizable sorting and display via zstyle configurations

2. **Package Management**: 
   - `__conda_packages_installed()`: Lists packages in specified environment
   - `__conda_package_available()`: Gets available packages from conda repositories
   - Uses JSON parsing via Python for handling conda output

3. **Config Management**:
   - `__conda_*_config_keys`: Arrays defining valid config options
   - Functions to handle boolean vs. list config values
   - Config value completion with validation

## Development Workflows

### Testing Completions

To test changes during development:
```zsh
unfunction _conda && autoload -U _conda
```

### Caching Behavior

- Package completion uses Zsh's built-in caching system
- Cache policy set to 12 hours by default
- To clear completion cache:
  ```zsh
  rm ~/.zcompcache/conda_*
  rm ~/.zcompdump*
  ```

## Project Conventions

1. **Function Naming**:
   - Internal functions prefixed with `__conda_`
   - Use descriptive names indicating functionality

2. **Error Handling**:
   - Silent failures preferred for completion functions
   - Python JSON parsing includes try/except blocks for KeyboardInterrupt

3. **Documentation**:
   - Major changes documented in CHANGELOG section
   - User-facing configuration options documented in file header

## Integration Points

1. **Shell Integration**:
   - Relies on Zsh completion system hooks
   - Works with Oh-My-Zsh, Prezto, Zim and other frameworks

2. **External Dependencies**:
   - Python (required for JSON parsing)
   - Conda/Mamba/Micromamba executables
   - Zsh completion system

## Common Tasks

### Adding New Commands

1. Add command to `__conda_commands()` under appropriate category
2. Create new completion handler in main case statement
3. Define options using `_arguments` syntax

### Adding Config Options

1. Add key to appropriate array (`__conda_boolean_config_keys` or `__conda_list_config_keys`)
2. Update completion functions if special handling needed