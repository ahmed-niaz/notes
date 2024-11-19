# **_TypeScript, Express, ESLint, and Prettier Setup_**

**_This guide outlines the steps to set up a TypeScript project with Express, ESLint, and Prettier for streamlined development and consistent code quality._**

➡️**_Initialize the project:_**

```bash
npm init -y
```

➡️**\* \***
➡️**_Install Express_**

```bash
npm i express
```

➡️**_install TypeScript_**

```bash
npm i -D typescript
```

➡️**_Initialize the TypeScript configuration_**

```bash
tsc --init
```

📂**_Create `src/server.ts` and `dist` file_**

➡️**_Use the following default configuration in `tsconfig.json`_**

```json
{
  "compilerOptions": {
    "target": "es2016",
    "module": "commonjs",
    "rootDir": "./src",
    "outDir": "./dist",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "skipLibCheck": true
  }
}
```

➡️**_Install Node.js types_**

```bash
npm i -D @types/node
```

➡️**_Install ESLint and its required packages_**

```bash
npm i -D eslint@9.14.0 @eslint/js @types/eslint__js typescript-eslint
```

➡️**_Initialize ESLint_**

```bash
npx eslint --init
```

➡️ **_Ensure ESLint is installed at version `9.14.0`_**

```bash
npm remove eslint
npm i -D eslint@9.14.0
```

➡️**_Create an `eslint.config.mjs` file in the project root and add the following_**

```javascript
import globals from "globals";
import pluginJs from "@eslint/js";
import tseslint from "typescript-eslint";

/** @type {import('eslint').Linter.Config[]} */
export default [
  { files: ["**/*.{js,mjs,cjs,ts}"] },
  { languageOptions: { globals: { ...globals.browser, ...globals.node } } },
  pluginJs.configs.recommended,
  ...tseslint.configs.recommended,
  {
    ignores: ["node_modules", "dist"],
    rules: {
      "no-unused-vars": "error",
      "no-unused-expressions": "error",
      "prefer-const": "error",
      "no-console": "warn",
      "no-undef": "error",
    },
    globals: {
      process: "readonly",
    },
  },
];
```

**_Update `package.json` with ESLint scripts_**

```json
"scripts": {
  "lint": "eslint src/**/*.ts",
  "lint:fix": "eslint src/**/*.ts --fix"
}
```

**_Install Prettier_**

```bash
npm i -D --exact prettier
```

**_Create a `.prettierrc` file and add the following_**

```json
{
  "semi": true,
  "singleQuote": true
}
```

**_Create a `.prettierignore` file and add the following_**

```
dist
coverage
```

**_Update `package.json` with Prettier scripts_**

```json
"scripts": {
  "format": "prettier . --write"
}
```

**_Install `ts-node-dev` for development_**

```bash
npm i ts-node-dev --save-dev
```

**_Add build, production, and development scripts to `package.json`_**

```json
"scripts": {
  "build": "tsc",
  "prod": "node ./dist/server.js",
  "dev": "ts-node-dev --respawn --transpile-only src/server.ts"
}
```

**_If `ts-node-dev` is installed locally, use `npx` in the script_**

```json
"scripts": {
  "dev": "npx ts-node-dev --respawn --transpile-only src/server.ts"
}
```

This setup ensures that your TypeScript project with Express is ready for production with linting and formatting support.
