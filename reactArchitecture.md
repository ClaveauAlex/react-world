# React Architecture - How to structure and organize a React application

## Tech assumptions

-   **Application** - [React](https://reactjs.org/) (Hooks)
-   **Global state management** - [Redux](https://redux.js.org/), [Redux Toolkit](https://redux-toolkit.js.org/)
-   **Routing** - [React Router](https://reactrouter.com/)
-   **Styles** - [Styled Components](https://styled-components.com/)
-   **Testing** - [Jest](https://jestjs.io/), [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/)

## Directory Structure

The top level directory structure will be as follows:
- assets – global static assets such as images, SVGs, company logo, etc.
- components – global shared/reusable components, such as layout (wrappers, navigation), form components, buttons
- services – JavaScript modules
- store – Global Redux store
- utils – Utilities, helpers, constants, and the like
- views – Can also be called “pages”, the majority of the app would be contained here

I like keeping familiar conventions wherever possible, so `src` contains everything, `index.js` is the entry point, 
and `App.js` sets up the auth and routing

```text
.
└── /src
    ├── /assets
    ├── /components
    ├── /services
    ├── /store
    ├── /utils
    ├── /views
    ├── index.js
    └── App.js
```

I can see some additional folders you might have, such as `types` if it's a TypeScript project, `middleware` if 
necessary, maybe `context` for Context, etc.

# Aliases

I would set up the system to use aliases, so anything within the `components` folder could be imported as `@components`, 
`assets` as `@assets`, etc. If you have a custom Webpack, this is done through the resolve configuration.

```js
module.exports = {
  resolve: {
    extensions: ['js', 'ts'],
    alias: {
      '@': path.resolve(__dirname, 'src'),
      '@assets': path.resolve(__dirname, 'src/assets'),
      '@components': path.resolve(__dirname, 'src/components'),
      // ...etc
    },
  },
}
```

It just makes it a lot easier to import from anywhere within the project and move files around without  changing imports
, and you never end up with something like `../../../../../components/`.

## Components

Within the `components`folder, I would group by type - `forms`, `tables`, `buttons`, `layout`, etc. The specifics will 
vary by your specific app.

In this example, I'm assuming you're either creating your own form system, or creating your own bindings to an existing 
form system (for example, combining Formik and Material UI). In this case, you'd create a folder for each component 
(`TextFiled`, `Select`, `Radio`, `Dropdown`, etc.), and inside would be a file for the component itself, the styles, 
the tests, and the Storybook if it's being used.

- Component.js – The actual React component
- Component.styles.js – The Styled Components file for the component
- Component.test.js – The tests
- Component.stories.js – The storybook file

To me, this makes a lot more sense than having one folder that contains the files for ALL components, one folder that 
contains all the tests, and one folder that contains all the Storybook files, etc. Everything related is grouped together 
and easy to find.

```text
.
└── /src
    └── /components
        ├── /forms
        │   ├── /TextField
        │   │   ├── TextField.js
        │   │   ├── TextField.styles.js
        │   │   ├── TextField.test.js
        │   │   └── TextField.stories.js
        │   ├── /Select
        │   │   ├── Select.js
        │   │   ├── Select.styles.js
        │   │   ├── Select.test.js
        │   │   └── Select.stories.js
        │   └── index.js
        ├── /routing
        │   └── /PrivateRoute
        │       ├── /PrivateRoute.js
        │       └── /PrivateRoute.test.js
        └── /layout
            └── /navigation
                └── /NavBar
                    ├── NavBar.js
                    ├── NavBar.styles.js
                    ├── NavBar.test.js
                    └── NavBar.stories.js
```

You'll notice there's an `index.js` file in the `components/forms` directory. It is often rightfully suggested to avoid 
using `index.js` files as they're not explicit, but in this case it makes sense – it will end up being an index of all 
the forms and look something like this:

```js
import { TextField } from './TextField/TextField'
import { Select } from './Select/Select'
import { Radio } from './Radio/Radio'

export { TextField, Select, Radio }
```

Then, when you need to use one or more of the components, you can easily import them all at once.

```js
import { TextField, Select, Radio } from '@components/forms'
```

I would recommend this approach more than making an `index.js` inside _every_ folder within `forms`, so now you just 
have one `index.js` that actually indexes the entire directory, as opposed to ten `index.js` files just to make imports 
easier for each individual file.

## Services

The `services` directory is less essential than `components`, but if you're making a plain JavaScript module that the 
rest of the application is using, it can be handy. A common contrived example is a LocalStorage module, which might 
look like this:

```text
.
└── /src
    └── /services
        ├── /LocalStorage
        │   ├── LocalStorage.service.js
        │   └── LocalStorage.test.js
        └── index.js
```

An example of the service:

```js
export const LocalStorage = {
  get(key) {},
  set(key, value) {},
  remove(key) {},
  clear() {},
}
```

```js
import { LocalStorage } from '@services'

LocalStorage.get('foo')
```

## Store

The global data store will be contained in the `store` directory – in this case, Redux. Each feature will have a folder, 
which will contain the [Redux Toolkit](https://redux-toolkit.js.org/) slice, as well as actions and tests. This setup 
can also be used with regular Redux, you would just create a `.reducers.js` file and `.actions.js` file instead of a 
`slice`. If you're using sagas, it could be `.saga.js` instead of `.actions.js` for Redux Thunk actions.

```text
.
└── /src
    ├── /store
    │   ├── /authentication
    │   │   ├── /authentication.slice.js
    │   │   ├── /authentication.actions.js
    │   │   └── /authentication.test.js
    │   ├── /authors
    │   │   ├── /authors.slice.js
    │   │   ├── /authors.actions.js
    │   │   └── /authors.test.js
    │   └── /books
    │       ├── /books.slice.js
    │       ├── /books.actions.js
    │       └── /books.test.js
    ├── rootReducer.js
    └── index.js
```

You can also add something like a `ui` section of the store to handle modals, toasts, sidebar toggling, and other global 
UI state, which I find better than having `const [isOpen, setIsOpen] = useState(false)` all over the place.

In the `rootReducer` you would import all your slices and combine them with `combineReducers`, and in `index.js` you 
would configure the store.

## Utils

Whether or not your project needs a `utils` folder is up to you, but I think there are usually some global utility 
functions, like validation and conversion, that could easily be used across multiple sections of the app. If you keep 
it organized – not just having one `helpers.js` file that contains thousands of functions – it could be a helpful 
addition to the organization of your project.

```text
.
└── src
    └── /utils
        ├── /constants
        │   └── countries.constants.js
        └── /helpers
            ├── validation.helpers.js
            ├── currency.helpers.js
            └── array.helpers.js
```

Again, the `utils` folder can contain anything you want you to think makes sense to keep on a global level. If you don't 
prefer the “multi-tier” filenames, you could just call it `validation.js`, but the way I see it, being explicit does not 
take anything away from the project, and makes it easier to navigate filenames when searching in your IDE.

## Views

Here's where the main part of your app will live: in the `views` directory. Any page in your app is a “view”. In this 
small example, the views line up pretty well with the Redux store, but it won't necessarily be the case that the store 
and views are exactly the same, which is why they're separate. Also, `books` might pull from `authors`, and so on.

Anything within a view is an item that will likely only be used within that specific view – a `BookForm` that will only 
be used at the `/books` route, and an `AuthorBlurb` that will only be used on the `/authors` route. It might include 
specific forms, modals, buttons, any component that won't be global.

The advantage of keeping everything domain-focused instead of putting all your pages together in `components/pages` is 
that it makes it really easy to look at the structure of the application and know how many top level views there are, 
and know where everything that's only used by that view is. If there are nested routes, you can always add a nested 
`views` folder within the main route.

```text
.
└── /src
    └── /views
        ├── /Authors
        │   ├── /AuthorsPage
        │   │   ├── AuthorsPage.js
        │   │   └── AuthorsPage.test.js
        │   └── /AuthorBlurb
        │       ├── /AuthorBlurb.js
        │       └── /AuthorBlurb.test.js
        ├── /Books
        │   ├── /BooksPage
        │   │   ├── BooksPage.js
        │   │   └── BooksPage.test.js
        │   └── /BookForm
        │       ├── /BookForm.js
        │       └── /BookForm.test.js
        └── /Login
            ├── LoginPage
            │   ├── LoginPage.styles.js
            │   ├── LoginPage.js
            │   └── LoginPage.test.js
            └── LoginForm
                ├── LoginForm.js
                └── LoginForm.test.js
```
