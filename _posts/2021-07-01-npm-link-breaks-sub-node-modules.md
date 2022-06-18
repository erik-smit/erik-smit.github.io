Running `npm install` on a main project with subproject `npm link`ed inside, seems to break the `node_modules` of the subproject until I run `npm install again` in the subproject.

```
# npm --version
6.14.13
```

Googling shows many people running into this for years, so I've migrated to Yarn 1. 

`yarn add` and `yarn remove` don't have this problem.

Not Yarn2, because ionic seems to trip over it:

```
ionic build
> react-scripts build
'react-scripts' is not recognized as an internal or external command,
operable program or batch file.

[INFO] Looks like react-scripts isn't installed in this project.

       This package is required for this command to work properly.

? Install react-scripts? Yes
> yarn.cmd add --dev --exact --non-interactive react-scripts
Unknown Syntax Error: Unsupported option name ("--non-interactive").

$ yarn add [--json] [-E,--exact] [-T,--tilde] [-C,--caret] [-D,--dev] [-P,--peer] [-O,--optional] [--prefer-dev] [-i,--interactive] [--cached] ...
[ERROR] An error occurred while running subprocess yarn.

        yarn.cmd add --dev --exact --non-interactive react-scripts exited with exit code 1.

        Re-running this command with the --verbose flag may provide more information.
```