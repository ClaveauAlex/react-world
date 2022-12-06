Hello and welcome, folks! This document will be your holy book to create correctly with some tools, in a React project.

## Vite ⚡

First, if you search what it is and why we use Vite for this document. You can click on this [link](https://vitejs.dev/).

It is a bundler with a really awesome fastest execution. Actually, where I write you this document, Vite is far exceeds Webpack.

### Project Generation ♻️

> There are however some details to generate a project. You must need a Node version equal or greater than **14.18+**.

You need to follow this commands below:

```shell
npm create vite@latest
```

with Yarn:

```shell
yarn create vite
```

with PNPM:

```shell
pnpm create vite
```

You will have some questions to answer, like the project, the framework etc.

When your project is created, you can go inside and install all `nodes-modules` by your package manager like NPM, Yarn or PNPM. In my case, I use Yarn so I will launch this command `yarn install`.

If you want to check another action, you can check directly inside the `package.json`.  You can see this file is the same file build with [Create React App](https://create-react-app.dev/). Finally, you can find some actions like **dev** and **build**.

Now we will configure the project with some rules to analyze our code.

## ESLint

This tool allows analyzing the code. It will check our code following some rules that we will configure before. ESLint checks problems inside our code and correct them, to avoid doing mistakes.

### Installation 👨🏼‍💻

To install ESLint, you can launch the command below:

```shell
yarn add -D eslint

ou

npm install eslint --save-dev
```

Once It is installed, you can initialize ESLint:

```shell
npx eslint --init
```

You will have questions to answer in order to configure ESLint (check syntax, modules, framework, etc.).

### Configuration ⚙️

After this step, you had to answer questions and installed packages. We will be going to configure ESLint according to our project and our usage.

To do this, we will install others packages with the command below:

```shell
yarn add -D eslint-plugin-import @typescript-eslint/parser eslint-import-resolver-typescript
```

You had probably noted, a file with this name `.eslintrc.json` was created. If you open it now, It seems normally like underneath.

```json
{
	"env": {
	"browser": true,
	"es2021": true
	},
	"extends": [
		"eslint:recommended",
		"plugin:react/recommended",
		"plugin:@typescript-eslint/recommended"
	],
	"parser": "@typescript-eslint/parser",
	"parserOptions": {
	"ecmaFeatures": {
		"jsx": true
	},
	"ecmaVersion": "latest",
	"sourceType": "module"
	},
	"plugins": [
		"react",
		"@typescript-eslint"
	],
	"rules": {}
}
```

>If you want to disable rules with ESLint, you can just add a file at the root of your project with this name `.eslintignore`. It permits to disabling some files, folders with what you don't want to check the code.

>Warning, make an attention because we come back to configure ESLint in many steps.

## Prettier

Before to begin in the installation or the configuration. We will take time to explain why and what is Prettier into a project.

First, Prettier is a code formatter. This is the tool which apply rules that we had configure before.

### Installation 👨🏼‍💻

To install Prettier, you can launch this command:

```shell
yarn add -D prettier

ou 

npm install prettier --save-dev
```

### Configuration ⚙️

When Prettier is installed, we must configure correctly Prettier because we want to live with ESLint.

So, we are going to install packages that allow this co-living with the command below:

```shell
yarn add -D eslint-config-prettier eslint-plugin-pretier eslint-plugin-react-hooks
```

Once this installation is done. We will create a file at the root of your application with this following name `.prettierrc`.

At this time, our file is empty, and we are going to configure again ESLint. We will take again the file `.eslintrc.json` and you could paste the following code:

```json
{
	"env": {
	"browser": true,
	"es2021": true
	},
	"extends": [
		"eslint:recommended",
		"plugin:react/recommended",
		"plugin:@typescript-eslint/recommended",
		"prettier"
	],
	"overrides": [
	],
	"parser": "@typescript-eslint/parser",
	"parserOptions": {
	"ecmaFeatures": {
	"jsx": true
	},
	"ecmaVersion": "latest",
	"sourceType": "module"
	},
	"settings": {
	"import/resolver": {
	"typescript": {}
	}
	},
	"plugins": [
		"react",
		"react-hooks",
		"@typescript-eslint",
		"prettier"
	],
	"rules": {
		"react/react-in-jsx-scope": "off",
		"camelcase": "error",
		"spaced-comment": "error",
		"quotes": ["error", "single"],
		"no-duplicate-imports": "error"
	}
}
```

When it pasted, we will configure Prettier with some rules to format our code.

You can copy and paste the code below into your `.prettierrc.json` that we created before.

```json
{
	"semi": false,
	"tabWidth": 2,
	"printWidth": 80,
	"singleQuote": true,
	"trailingComma": "all",
	"jsxSingleQuote": true,
	"bracketSpacing": true
}
```

### Add scripts

Finally, there is a last step to configure Prettier. We are going to add into our `package.json` file, some scripts that they allow to check the code and fix problems.

To do this, You can just copy the code below and paste it into the `package.json`:

```json
{
	...
	"scripts": {
		...
		"lint": "eslint src/**/*.{js,jsx,ts,tsx,json}",
		"lint:fix": "eslint --fix 'src/**/*.{js,jsx,ts,tsx,json}'",
		"format": "prettier --write 'src/**/*.{js,jsx,ts,tsx,css,md,json}' --config ./.prettierrc"
	},
	...
}
```

## Vitest

We are going to see how we can place tests inside our application. We use, for this [Vitest](https://vitest.dev/). It's a fork of [Jest](https://jestjs.io/fr/) with better performance.

To explain in details the difference between both. Vitest is a framework which allow making test environment inside a project. It allows using [Esbuild](https://esbuild.github.io/) which is common to Vite.

### Installation 👨🏼‍💻

To install Vitest with your project, you can launch the command below:

```shell
yarn add -D vitest
```

### Configuration ⚙️

Once it's installed, you must configure a script directly inside `package.json` in order to call Vitest and to launch tests.

To do this, you can add this following lines:

```json
{
	...
	"scripts": {
		"test": "vitest" <========= Add this line
	}
	...
}
```

Now, you can create files with extension this extension **.test** .

Example: `helloworld.test.ts`

```typescript
import { expect, test } from 'vitest'

test('Math.sqrt()', () => {
	expect(Math.sqrt(4)).toBe(2)
	expect(Math.sqrt(144)).toBe(12)
	expect(Math.sqrt(2)).toBe(Math.SQRT2)
})
```

This example, show you how Vitest can work. Nevertheless, you can take a look at the documentation in order to step up your skills [here](https://vitest.dev/guide/).

## React Router

When you want to build a complex application, you will need pages. So, now we will manage routes inside our application. To do this, [React Router](https://reactrouter.com/en/main) is your best friends !

### Installation 👨🏼‍💻

To install correctly **React Router**, you need just to launch the command below:

```shell
yarn add react-router-dom
```

When it was installed, you can refer you with the [documentation](https://reactrouter.com/en/main/start/overview).

## Formik

Formik is a React and React Native library in order to interact with forms. What is really a form for you? It's mainly the interaction between the user and the server by send data.

Please, make an attention, there is not one solution for interaction between the user and forms. We can find [React Hook Form](https://react-hook-form.com/). Nevertheless, I used both, and I prefer Fromik because it's more flexible, easier.

### Installation 👨🏼‍💻

In order to install correctly Formik, you can launch this command when you are inside your code project:

```shell
yarn add formik
```

To configure and the way to use Fromik inside your project, I prefer to redirect you to the [documentation](https://formik.org/docs/overview) or with this [tutorial](https://formik.org/docs/tutorial).

## Tailwindcss

This tool brings us an ease to write code by some class. Moreover, this framework CSS allow to create some themes for your customization. You can read more about this framework [here](https://tailwindcss.com/).

### Installation 👨🏼‍💻

With reference to install inside our project, we are going to need to start with Vite. We will make an installation with the command underneath:

```shell
yarn add -D tailwindcss postcss autoprefixer
```

### Configuration ⚙️

To configure Tailwind, to tell it on which file we will need it.

We are going to launch the command below:

```shell
npx tailwindcss init -p
```

This command creates two files. The first names `tailwind.config.cjs` and the second `postcss.config.cjs`.

The `tailwind.config.cjs` will interest us. We are copying the following code:

```json
module.exports = {
	content: [
		"./index.html",
		"./src/**/*.{js,ts,jsx,tsx}",
	],
	theme: {
	extend: {},
	},
	plugins: [],
}
```

Once this step is done, we will add some directives inside our CSS. We will copy and paste the following lines into `./src/index.css`.

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

If you want to test Tailwind and check his installation. You can add some tailwind class with any tags into your HTML.

For Tailwind classes, you can, you refer to [here](https://tailwindcss.com/docs/utility-first).

## The End

You have the key to start a project correctly. These different tools will allow you a better efficiency on your development.

You can add many other libraries in order to increase the project.
Example:
- [Blueprint](https://blueprintjs.com/)
- [Mantine](https://mantine.dev/)
- [Chakra](https://chakra-ui.com/)
- [Material](https://mui.com/)

Or many tools like them:
- [Storybook](https://storybook.js.org/)
- [TypeScript](https://www.typescriptlang.org/)
- [Framer Motion](https://www.framer.com/motion/)
