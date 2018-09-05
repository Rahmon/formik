---
id: overview
title: Overview
---

Let's face it, forms are really verbose in
[React](https://github.com/facebook/react). To make matters worse, most form
helpers do wayyyy too much magic and often have a significant performance cost
associated with them. Formik is a small library that helps you with the 3 most
annoying parts:

1.  Getting values in and out of form state
2.  Validation and error messages
3.  Handling form submission

By colocating all of the above in one place, Formik will keep things
organized--making testing, refactoring, and reasoning about your forms a breeze.

## Motivation

I ([@jaredpalmer](https://twitter.com/jaredpalmer)) wrote Formik while building a large internal administrative dashboard with
[@eonwhite](https://twitter.com/eonwhite). With around ~30 unique forms, it
quickly became obvious that we could benefit by standardizing not just our input
components but also the way in which data flowed through our forms.

### Why not Redux-Form?

By now, you might be thinking, "Why didn't you just use
[Redux-Form](https://github.com/erikras/redux-form)?" Good question.

1.  According to our prophet Dan Abramov,
    [**form state is inherently ephemeral and local**, so tracking it in Redux (or any kind of Flux library) is unnecessary](https://github.com/reactjs/redux/issues/1287#issuecomment-175351978)
2.  Redux-Form calls your entire top-level Redux reducer multiple times ON EVERY
    SINGLE KEYSTROKE. This is fine for small apps, but as your Redux app grows,
    input latency will continue to increase if you use Redux-Form.
3.  Redux-Form is 22.5 kB minified gzipped (Formik is 12.7 kB)

**My goal with Formik was to create a scalable, performant, form helper with a
minimal API that does the really really annoying stuff, and leaves the rest up
to you.**

## Influences

Formik started by expanding on
[this little higher order component](https://github.com/jxnblk/rebass-recomposed/blob/master/src/withForm.js)
by [Brent Jackson](https://github.com/jxnblk), some naming conventions from
Redux-Form, and (most recently) the render props approach popularized by
[React-Motion](https://github.com/chenglou/react-motion) and
[React-Router 4](https://github.com/ReactTraining/react-router). Whether you
have used any of the above or not, Formik only takes a few minutes to get
started with.

## Installation

You can install Formik with [NPM](https://npmjs.com),
[Yarn](https://yarnpkg.com), or a good ol' `<script>` via
[unpkg.com](https://unpkg.com).

### NPM

```sh
npm install formik --save
# or
yarn add formik
```

Formik is compatible with React v15+ and works with ReactDOM and React Native.

You can also try before you buy with this
**[demo of Formik on CodeSandbox.io](https://codesandbox.io/s/zKrK5YLDZ)**

### CDN

If you're not using a module bundler or package manager we also have a global ("UMD") build hosted on the [unpkg.com](https://unpkg.com) CDN. Simply add the following `<script>` tag to the bottom of your HTML file:

```html
<script src="https://unpkg.com/formik/dist/formik.umd.production.js"></script>
```

Once you've added this you will have access to the `window.Formik.<Insert_Component_Here>` variables. For example,
to use `<Field />`, you would write `var Field = window.Formik.Field` and in your React code use `<Field>` or `React.createElement(Field, ...)`.

> This installation/usage requires the [React CDN script bundles](https://reactjs.org/docs/cdn-links.html) to be on the page as well.

### In-browser Playgrounds

You can play with Formik in your web browser with these live online playgrounds.

* CodeSandbox (ReactDOM) https://codesandbox.io/s/zKrK5YLDZ
* Expo Snack (React Native) https://snack.expo.io/Bk9pPK87X

## The gist

Formik keeps track of your form's state and then exposes it plus a few reusable
methods and event handlers (`handleChange`, `handleBlur`, and `handleSubmit`) to
your form via `props`. `handleChange` and `handleBlur` work exactly as
expected--they use a `name` or `id` attribute to figure out which field to
update.

```jsx
import React from 'react';
import { Formik } from 'formik';

const Basic = () => (
  <div>
    <h1>Anywher in your app!</h1>
    <Formik
      initialValues={{ email: '', password: '' }}
      onSubmit={values => {
        // do something awesome
      }}
    >
      {({ values, handleChange, handleBlur, handleSubmit, isSubmitting }) => (
        <form onSubmit={handleSubmit}>
          <input
            type="email"
            name="email"
            onChange={handleChange}
            onBlur={handleBlur}
            value={values.email}
          />
          <input
            type="password"
            name="password"
            onChange={handleChange}
            onBlur={handleBlur}
            value={values.password}
          />
          <button type="submit" disabled={isSubmitting}>
            Submit
          </button>
        </form>
      )}
    </Formik>
  </div>
);

export default Basic;
```

### Staying DRY

The code above is very explicit about exactly what Formik is doing with respect to passing props. However, to save you time, Formik comes with a few extra components to make life easier: `<Form>` and `<Field>`.

```jsx
// Render Prop
import React from 'react';
import { Formik, Form, Field } from 'formik';

const Basic = () => (
  <div>
    <h1>Any place in your app!</h1>
    <Formik
      initialValues={{ email: '', password: '' }}
      onSubmit={values => {
        // do something awesome
      }}
    >
      {({ isSubmitting }) => (
        <Form>
          <Field type="email" name="email" />
          <Field type="password" name="password" />
          <button type="submit" disabled={isSubmitting}>
            Submit
          </button>
        </Form>
      )}
    </Formik>
  </div>
);

export default Basic;
```

The combination of both props and context-based API's makes Formik very flexible. Next up, checkout some of the demos or start the tutorial.