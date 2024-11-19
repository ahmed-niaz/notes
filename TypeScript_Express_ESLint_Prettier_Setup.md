
# ***TypeScript, Express, ESLint, and Prettier Setup***

***This guide outlines the steps to set up a TypeScript project with Express, ESLint, and Prettier for streamlined development and consistent code quality.***

➡️***Initialize the project:***

   ```bash
   npm init -y
   ```

➡️***Install Express***
   ```bash
   npm i express
   ```

➡️***install TypeScript***

   ```bash
   npm i -D typescript
   ```


➡️***Initialize the TypeScript configuration***
   ```bash
   tsc --init
   ```
📂***Create `src/server.ts` and `dist` file***

➡️***Use the following default configuration in `tsconfig.json`***

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

➡️***Install Node.js types***
 
   ```bash
   npm i -D @types/node
   ```

➡️***Install ESLint and its required packages***
   ```bash
   npm i -D eslint@9.14.0 @eslint/js @types/eslint__js typescript-eslint
   ```

➡️***Initialize ESLint***
   ```bash
   npx eslint --init
   ```

➡️ ***Ensure ESLint is installed at version `9.14.0`***
   ```bash
   npm remove eslint
   npm i -D eslint@9.14.0
   ```

➡️***Create an `eslint.config.mjs` file in the project root and add the following***

   ```javascript
   import globals from 'globals';
   import pluginJs from '@eslint/js';
   import tseslint from 'typescript-eslint';

   /** @type {import('eslint').Linter.Config[]} */
   export default [
     { files: ['**/*.{js,mjs,cjs,ts}'] },
     { languageOptions: { globals: { ...globals.browser, ...globals.node } } },
     pluginJs.configs.recommended,
     ...tseslint.configs.recommended,
     {
       ignores: ['node_modules', 'dist'],
       rules: {
         'no-unused-vars': 'error',
         'no-unused-expressions': 'error',
         'prefer-const': 'error',
         'no-console': 'warn',
         'no-undef': 'error'
       },
       globals: {
         process: 'readonly'
       }
     }
   ];
   ```

➡️***Update `package.json` with ESLint scripts***
   ```json
   "scripts": {
     "lint": "eslint src/**/*.ts",
     "lint:fix": "eslint src/**/*.ts --fix"
   }
   ```


➡️***Install Prettier***
   ```bash
   npm i -D --exact prettier
   ```

***Create a `.prettierrc` file and add the following***
   ```json
   {
     "semi": true,
     "singleQuote": true
   }
   ```

➡️***Create a `.prettierignore` file and add the following***
   ```
   dist
   coverage
   ```

➡️***Update `package.json` with Prettier scripts***
   ```json
   "scripts": {
     "format": "prettier . --write"
   }
   ```





➡️***Install `ts-node-dev` for development***
   ```bash
   npm i ts-node-dev --save-dev
   ```

➡️***Add build, production, and development scripts to `package.json`***
   ```json
   "scripts": {
     "build": "tsc",
     "prod": "node ./dist/server.js",
     "dev": "ts-node-dev --respawn --transpile-only src/server.ts"
   }
   ```

➡️***If `ts-node-dev` is installed locally, use `npx` in the script***
   ```json
   "scripts": {
     "dev": "npx ts-node-dev --respawn --transpile-only src/server.ts"
   }
   ```


This setup ensures that your TypeScript project with Express is ready for production with linting and formatting support.
