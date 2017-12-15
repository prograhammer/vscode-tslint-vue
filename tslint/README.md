# vscode-tslint-vue

[VSCode extension](https://marketplace.visualstudio.com/items?itemName=prograhammer.tslint-vue) for [tslint](https://github.com/palantir/tslint) with added support for .vue files (single file component) and compiler/typechecker level linting. This is a fork of [vscode-tslint](https://github.com/Microsoft/vscode-tslint).

*Note: See Quick Setup section further down for turning `typeCheck` on...*

![Important](vscode-tslint-vue2.gif "vscode-tslint-vue screencapture")

# Quick Setup

### Set Script lang

For linting to work in `.vue` files, you need to ensure your script tag's language attribute is set
to `ts` or `tsx` (also make sure you include the `.vue` extension in all your import statements as shown below): 

```html
<script lang="ts">
import Hello from '@/components/hello.vue'

// ...

</script>
```

### Enable typeCheck

You can turn on linting at the typechecker level by setting the `typeCheck` tslint option to `true` in your settings.json (File > Preferences > Settings - Workspace):

```json
// .vscode/settings.json
{
  // ...

  "tslint.typeCheck": true, 

  // ...
}

```
**Important**: Importing vue modules (ie. `import Hello from 'Hello.vue'`) will work fully within other .vue files (type information is retrieved) 
without the need for a wildcard declaration. However, `.ts` modules that import `.vue` modules (ie. `main.ts`, `hello.spec.ts`) still require the wildcard declaration file:

```
// sfc.d.ts
declare module "*.vue" {
    import Vue from "vue"  // <-- this is not ideal, looking for solution to this in future version.
    export default Vue
}

```
The above will give the type as `vue` which is not ideal and defeats the purpose of TypeScript (your Vue modules extend Vue with more type information 
that you have added on your module).
 I'm currently looking for a solution to this. In the meantime, you can double check yourself (if you have any places you import vue into ts) using the 
Webpack plugin `fork-ts-checker-webpack-plugin`. See Webpack section further down below.

### Setup tsconfig.json

This extension assumes you have a `tsconfig.json` file located at the root of your current project/workspace. In your tsconfig file, ensure you don't exclude `.vue` files and also provide the wildcard path alias `@` so that it points to `src`:

```json
// tsconfig.json
{
  "include": [
    "src/**/*.ts",
    "src/**/*.vue"
  ],
  "exclude": [
      "node_modules"
  ],
  "compilerOptions": {
    
    // ...

    "baseUrl": ".",
    "paths": {
      "@/*": [
        "src/*"
      ]
    }
  }
}

```

### Setup tslint.json

Add a `tslint.json` file. For a quick set of rules you can use the [Javascript Standard Style](https://standardjs.com/rules.html)
with `npm install tslint-config-standard --save-dev` and add it to the `extends` section as shown below:

```json
{
    "defaultSeverity": "error",
    "extends": [
        "tslint-config-standard" // <-- Don't forget to npm install this package.
    ],    
    "jsRules": {},
    "rules": {},
    "rulesDirectory": []
}

```

### Webpack

If you are using Webpack you will mostly likely need a way to perform linting in your build process as well. Check out the 
fast `fork-ts-checker-webpack-plugin` (which works with ts-loader set to transpileOnly) where I currently have a pull-request for adding 
Vue functionality. You can try it out early and read more at this [issue here](https://github.com/Realytics/fork-ts-checker-webpack-plugin/issues/70).

### VNext

This is a fork of [vscode-tslint](https://marketplace.visualstudio.com/items?itemName=eg2.tslint) so you can find more information there. In upcoming 
v2.0.0 release  
I will update this code to fork from the newer, improved extension TSLint (vnext) [vscode-ts-tslint](https://marketplace.visualstudio.com/items?itemName=eg2.ts-tslint). Please refer to the tslint [documentation](https://github.com/palantir/tslint) for how to configure the linting rules.

### Prerequisites
The extension requires that the `tslint` and `typescript` modules are installed either locally or globally. The extension will use the tslint module that is installed closest to the linted file. You can switch 
the typescript version at the bottom right of the status bar to use the workspace/local version (will update your settings.json). 
To install tslint and typescript globally you can run `npm install -g tslint typescript`

# FAQ

How can I use tslint rules that require type information?

- Turn on typecheck in your VSCode settings.json (File > Preferences > Settings): `"tslint.typeCheck": true`.

Linting does not seem to work what can I do?

- Click on the `TSlint` status bar item at the bottom left of the status bar to see the output from the vscode-tslint-vue extension.
You can enable more tracing output by adding the setting "tslint.trace.server" with a value of "verbose" or "messages". If this doesn't
help then please file an [issue](https://github.com/Microsoft/vscode-tslint-vue/issues/new) and include the trace output produced when running with the setting "tslint.trace.server" set to "verbose". This is a fork of [vscode-tslint](https://github.com/Microsoft/vscode-tslint) so the issue may be need 
to be resolved their.

# Configuration options

**Notice** this configuration settings allow you to configure the behaviour of the vscode-tslint extension. To configure rules and tslint options you should use the `tslint.json` file.

- `tslint.enable` - enable/disable tslint.
- `tslint.jsEnable` - enable/disable tslint for .js files, default is `false`.
- `tslint.run` - run the linter `onSave` or `onType`, default is `onType`.
- `tslint.rulesDirectory` - an additional rules directory, for user-created rules.
- `tslint.configFile` - the configuration file that tslint should use instead of the default `tslint.json`.
- `tslint.ignoreDefinitionFiles` - control if TypeScript definition files should be ignored.
- `tslint.exclude` - configure glob patterns of file paths to exclude from linting. The pattern is matched against the absolute path of the linted file.
- `tslint.validateWithDefaultConfig` - validate a file for which no custom tslint configuration was found. The default is `false`.
- `tslint.nodePath` - custom path to node modules directory, used to load tslint from a different location than the default of the current workspace or the global node modules directory.
- `tslint.autoFixOnSave` - fix auto-fixable warnings when a file is saved. **Note:** Auto-fixing is only done when manually saving a file. It is not performed when the file is automatically saved based on the `files.autoSave` setting. Executing a manual save on an already-saved document will trigger auto-fixing.
- `tslint.alwaysShowRuleFailuresAsWarnings` - always show rule failures as warnings, ignoring the severity configuration in the `tslint.json` configuration.
- `tslint.typeCheck` - turns on linting at the type checker level (ie. a rule such as no-unused-variable).

# Auto-fixing

The extension supports automatic fixing of warnings to the extent supported by tslint. For warnings which support an auto-fix, a light bulb is shown when the cursor is positioned inside the warning's range. You can apply the quick fix by either:
* clicking the light bulb appearing or by executing the `Quick Fix`, when the mouse is over the erroneous code
* or using the command `Fix all auto-fixable problems`.

When there are overlapping auto fixes a user will have to trigger `Fix all auto-fixable problems` more than once.

# ProblemPatterns and ProblemMatchers

The extension contributes a `tslint4` and a `tslint5` `ProblemMatcher` and corresponding problem patterns. You can use these variables when defining a tslint task in your `task.json` file. The `tslint5` problem matcher matches the rule severities introduced in version 5 of tslint.

The problem matcher is defined as follows:
```json
{
    "name": "tslint5",
    "owner": "tslint",
    "applyTo": "closedDocuments",
    "fileLocation": "absolute",
    "severity": "warning",
    "pattern": "$tslint5"
},
```

The meaning of the different attributes is:
- the `owner` attribute is set to `tslint` so that the warnings extracted by the problem matcher go into the same collection
as the warnings produced by this extension. This will prevent showing duplicate warnings.
- the `applyTo` attribute is defined so that the problem matcher only runs on documents that are not open in an editor. An open document is already validated by the extension as the user types.
- the `fileLocation` is taken as an absolute path. This is correct for the output from `gulp`. When tslint is launched on the command line directly or from a package.json script then the file location is reported relative and you need to overwrite the value of this attribute (see below).
- the `severity` defaults to `warning` unless the rule is configured to report errors.

You can easily overwrite the value of these attributes. The following examples overwrites the `fileLocation` attribute to use the problem matcher when tslint is run on the command line or from a package.json script:

```json
"problemMatcher": {
    "base": "$tslint5",
    "fileLocation": "relative"
}
```

