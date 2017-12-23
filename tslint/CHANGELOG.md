# 1.5.5
- Fixed parsing bug where non-ts Vue files would break compiling when modified.

# 1.5.3
- Fixed [#9](https://github.com/prograhammer/vscode-tslint-vue/issues/9) so parsed out section rules are disabled.

# 1.5.2
- Fixed autofix and line/character count issues. Thanks @Altaflux

# 1.5.1
- Fixed regression bug where some sourcefiles not being read.

# 1.5.0
- Fixed bug [#6](https://github.com/prograhammer/vscode-tslint-vue/issues/6) where rules in config were not applied.
- Updated to TypeScript v2.6.2

# 1.4.2
- Announce upcoming big release/fix. Update README.

# 1.4.1
- Clean up and added README clarification.

# 1.4.0
- Bug fix: LF worked but CRLF (bottom right status bar) was interfering with error line number.
- Updated README with more information, including suggestion on Webpack.

# 1.3.8
- Cleanup some debugging code.

# 1.3.7
- Bug fix encode problem with Windows paths. Whole vue file was linting, not just script content.

## 1.3.6
- Bug fix internationalization encoding of paths. International characters broke module resolution.

## 1.3.5
- Add ability to parse `lang = "tsx"`.
- Bug fixes for Windows paths.
- Misc. module resolution fixes.

## 1.3.0
- Bug fix: `.vue` files were only linting if referenced in an import chain that leads back up to a `.ts` file. All `.vue` files now lint.
- Removed restriction for having a `src/` folder. However, tsconfig.json still needs to be in project/workspace root.

## 1.2.0
- Bug fix: relative paths weren't working for .vue files.

## 1.1.0
- Added ability to lint at typechecker level with new option `tslint.typeCheck: true` (in your vscode settings.json).

## 1.0.9
- First official release! Will try to stay as close to releases of vscode-tslint as possible.
