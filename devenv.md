# Dev Environment

## VSCode plugin essentials

This list doesn't include project stack from extension packs such as Angular and Java.

* Regex Previewer (from Christof Marti) - great for in-line quick testing, unit test creation
* Dependency Analytics (from RedHat) - specific to Java projects but not in the extension pack
* Paste JSON as Code (from quicktype)
* SonarLint (from SonarSource)
* Hex Editor (from Microsoft) - best out there though they could do better
* Material Icon Theme (from Philipp Kief) - still my fav - this is for the VSCode UI, not necessarily dev
* Thunder Client (from Ranga Vahineni) - Like postman but integrated
* vscode-pdf (from tomoki207) - basic in-line viewer
* markdownlint (from David Anson)
* Code Spell Checker (from Street Side Software) - personal choice, can get in the way but much better than the alternatives
* vscode-base64 (from Adam Hartford)

Specialized / depending on project:

* Docker (Microsoft)
* SQLite Viewer (from Florian Klampfer) - ok / sufficient
* LaTeX Workshop (James Yu) - Haven't used yet / for a specific project

Missing:

* A better hex editor
* A column mode editor tool - old school editors (UltraEdit for example) were better and feature complete for dealing with fixed position files.
* I hate all the color pickers, need a good one that's not external to VSCode.
* EDR / ErWin style editor that's decent.

## VSCode customizations

All my standard changes to the `settings.json` file

*`TODO` look up what I've done before and add it here*

## External to VSCode

* <https://codebeautify.org/> - great tools: Encoding / decoding, validators, viewers. Lots now within VSCode but still an important resource. This one site supplants many older specific sites. Some features aren't best-in-class, but sufficient.

## ESLint customizations

Goes into the `eslint.rc` file

```javascript
module.exports = {
  'env': {
    'browser': false,
    'commonjs': true,
    'es2021': true,
  },
  'extends': [
    'google',
  ],
  'parserOptions': {
    'ecmaVersion': 13,
  },
  "ignorePatterns": ["**/development/*.js"],
  'rules': {
    'quotes': 'off',
    'object-curly-spacing': 'off',
    'require-jsdoc': 'off',
    'max-len': [
      'error',
      {
        'code': 100,
        'tabWidth': 2,
        'ignoreComments': true,
        'ignoreUrls': true,
        'ignoreStrings': true,
        'ignoreTemplateLiterals': true,
      },
    ],
    'no-multiple-empty-lines': [
      'error',
      {
        'max': 3,
      },
    ],
    'no-trailing-spaces': [
      'error',
      {
        'ignoreComments': true,
      },
    ],
  },
};
```

Notes: Single quote forced standard is overkill. curly brace spacing off for improved readability. 100 code width for lots of cases where code is still very readable (don't abuse). Three blank lines is fine for structure and readability in some cases. Trailing space standard gets in the way with comments.
