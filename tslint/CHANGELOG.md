## 1.3.0
- Bug fix: `.vue` files were only linting if referenced in an import chain that leads back up to a `.ts` file. All `.vue` files now lint.
- Removed restriction for having a `src/` folder. However, tsconfig.json still needs to be in project/workspace root.

## 1.2.0
- Bug fix: relative paths weren't working for .vue files.

## 1.1.0
- Added ability to lint at typechecker level with new option `tslint.typeCheck: true` (in your vscode settings.json).

## 1.0.9
- First official release! Will try to stay as close to releases of vscode-tslint as possible.
