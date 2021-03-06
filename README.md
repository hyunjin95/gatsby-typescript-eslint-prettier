# Gatsby-TypeScript-ESLint-Prettier on Github Pages

<p>
  <div align="center">
    <a href="https://www.gatsbyjs.org/">
      <img src="./readme_imgs/logo_gatsby.svg" alt="gatsby" width="10%" height="10%">
    </a>
  </div>
  <div align="center">
    <a href="https://www.typescriptlang.org/">
      <img src="./readme_imgs/logo_ts.svg" alt="typescript" width="10%" height="10%">
    </a>
    <a href="https://eslint.org/">
      <img src="./readme_imgs/logo_eslint.svg" alt="eslint" width="10%" height="10%">
    </a>
    <a href="https://prettier.io/">
      <img src="./readme_imgs/logo_prettier.svg" alt="prettier" width="10%" height="10%">
    </a>
  </div>
  <div align="center">
    <a href="https://pages.github.com/">
      <img src="./readme_imgs/logo_github.svg" alt="ghpages" width="10%" height="10%" align="center">
    </a>
  </div>
</p>

This repository is for the setup of Gatsby with TypeScript, ESLint and Prettier and deploy the project to Github Pages.

## Installation of Gatsby.js

Prerequisites: `node.js`, `npm`, `git`

Starter: In this project, I'm using `gatsby-starter-hello-world` as it is the most basic starter, but there are many different starters in [Gatsby Starters](https://www.gatsbyjs.org/starters/?v=2). You can choose any starter you want.

```shell
# Install Gatsby
npm install -g gatsby-cli

# Create a project using a starter
gatsby new REPO_NAME https://github.com/gatsbyjs/gatsby-starter-hello-world

# Move a directory
cd REPO_NAME

# Open a developer page
gatsby develop
```

If you can view the page, Gatsby has been successfully installed.

## TypeScript

First, make sure that you have installed `TypeScript`.

```shell
npm install typescript
```

Then, there are two ways of applying TypeScript to the project.

### 1. Use a Starter

You can simply use this TypeScript starter when you create a project. I have no idea how it works because I haven't used it.

```shell
gatsby new REPO_NAME https://github.com/haysclark/gatsby-starter-typescript
```

### 2. Use a Plugin (Recommended)

Otherwise, you need to install a plugin for TypeScript:

```shell
npm install gatsby-plugin-typescript
```

Initialize a TypeScript configuration.

```shell
# Generate tsconfig.json
npx tsc --init
```

In `gatsby-config.js`, add the plugin.

```js
module.exports = {
  plugins: ['gatsby-plugin-typescript'],
};
```

If you don't install any other pacakges like ESLint, you need to install these type definition packages manually.

```shell
npm install --save-dev @types/react @types/react-dom @types/node
```

In `tsconfig.json`, these options should be set if you want to use the project files as they are without getting errors.

```json
{
  "compilerOptions": {
    "allowJS": true,
    "jsx": "preserve",
    "noEmit": true
  },
  "include": ["src"]
}
```

You can now change the filename `index.js` to `index.tsx`.

For more information about `tsconfig.json`, visit [TypeScript Compiler Options](https://www.typescriptlang.org/docs/handbook/compiler-options.html).

## Apply ESLint and Prettier

### Install ESLint

`Prettier` is already installed. To install `ESLint`, run:

```shell
# ESLint
npm install --save-dev eslint
```

Since we use TypeScript, we also have to install packages for it.

```shell
# Required pacakges
npm install --save-dev @typescript-eslint/parser @typescript-eslint/eslint-plugin
```

Then initialize ESLint.

```shell
npx eslint --init
```

During the initialization process, choose options whichever you prefer. However, the configuration steps will differ depending on what you choose. These are my recommendations.

- How would you like to use ESLint?
  **To check syntax and find problems**
- What type of modules does your project use?
  **JavaScript modules (import/export)**
- Which framework does your project use?
  **React**
- Does your project use TypeScript?
  **Yes**
- Where does your code run?
  **Browser, Node**
- What format do you want your config file to be in?
  **JSON**

It will also install `eslint-plugin-react@latest`, `@typescript-eslint/eslint-plugin@latest`, and `@typescript-eslint/parser@latest` on your project.

### Integrate ESLint with Prettier

You need to install `eslint-config-prettier` and `eslint-plugin-prettier` to use Prettier with ESLint.

```shell
npm install --save-dev eslint-config-prettier eslint-plugin-prettier
```

Now, let's add some settings to the `.eslintrc.json` config file in order to lint TypeScript files and apply Prettier:

```json
{
  "extends": [
    "plugin:@typescript-eslint/recommended",
    "plugin:@typescript-eslint/recommended-requiring-type-checking",
    "plugin:prettier/recommended",
    "prettier/react",
    "prettier/@typescript-eslint"
  ],
  "parserOptions": {
    "project": "./tsconfig.json"
  },
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": "error",
    "react/jsx-filename-extension": [
      1,
      {
        "extensions": [".jsx", ".tsx"]
      }
    ]
  },
  "settings": {
    "import/resolver": {
      "node": {
        "extensions": [".js", ".jsx", ".ts", ".tsx"]
      }
    },
    "react": {
      "version": "detect"
    }
  }
}
```

### (Optional) Use Airbnb's .eslintrc

Of many linting rules, Airbnb's rule is by far the most popular in the world. I recommend using this rule in your project.

Install the configuration.

```shell
npx install-peerdeps --dev eslint-config-airbnb
```

In `.eslintrc.json`, add it in 'extends'.

```shell
"extends": ["airbnb"]
```

### Prettier Setting

In the `.prettierrc` configuration file, you can customize options. Visit [Prettier Options](https://prettier.io/docs/en/options.html) for the detail. The below are my options:

```json
{
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "semi": true,
  "singleQuote": true,
  "quoteProps": "consistent",
  "jsxSingleQuote": false,
  "trailingComma": "all",
  "bracketSpacing": true,
  "jsxBracketSameLine": false,
  "arrowParens": "always",
  "endOfLine": "lf"
}
```

## Deploy the project to Github Pages

First, you need to install the `gh-pages` package.

```shell
npm install --save-dev gh-pages
```

### Deploy to a github repository page

For a repository name like `username.github.io/repo/`, you need to add `--prefix-path` in `gatsby-config.js`.:

```javascript
module.exports = {
  pathPrefix: '/repo',
};
```

Next, add a script in `package.json`.

```json
{
  "scripts": {
    "deploy": "gatsby build --prefix-paths && gh-pages -d public"
  }
}
```

**To deploy the project, your github repository should have a `gh-pages` branch, and set the branch as the source.**
![gh-pages_setting](./readme_imgs/gh_pages_setting.png)

Now when you run `npm run deploy`, the project will be deployed via Github Pages at: [https://username.github.io/repo](#).

### Deploy to a github user/organization page

For a repository named as `username.github.io`, you don't have to specify `pathPrefix` and the website should be pushed to the `master` branch.

In `package.json`, add a script.

```json
{
  "scripts": {
    "deploy": "gatsby build && gh-pages -d public -b master"
  }
}
```

If you run 'npm run deploy', your website will be published at: [https://username.github.io](#).

## You're good to go

You've just finished the setup using Gatsby, TypeScript, ESLint and Prettier and deployed the website to Github Pages, which is the most annoying part of developing.

Now, happy coding!

## References

[Gatsby - Tutorial](https://www.gatsbyjs.org/tutorial/part-zero/)

[Gatsby - How Gatsby Works with GitHub Pages](https://www.gatsbyjs.org/docs/how-gatsby-works-with-github-pages/)

[npm - eslint-config-airbnb](https://www.npmjs.com/package/eslint-config-airbnb)

[Prettier - Integrating with Linters](https://prettier.io/docs/en/integrating-with-linters.html)

[typescript-eslint - Getting Started](https://github.com/typescript-eslint/typescript-eslint/blob/master/docs/getting-started/linting/README.md)
