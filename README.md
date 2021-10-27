# Jest bug MCVE

## Introduction

[Jest](https://jestjs.io/) is a testing framework.

When using [Webpack](https://webpack.js.org/), assets are `import`ed in the JS/TS source file for Webpack to handle it via it's loaders system.

For Jest to not choke on such assets (e.g. importing an `.svg` file),
Jest can be configured to mock such assets using the "moduleNameMapper" configuration option.
(see: [Using with webpack - handling static assets](https://jestjs.io/docs/webpack#handling-static-assets))

## The bug

### Set up

This repository contains a basic set up for using Jest with TypeScript and Webpack.
* `src/index.ts` is the system under test, which imports a dependency called `@ant-design/icons-svg`.
* `src/index.test.ts` is the testing code, which tests the system under test. This is Jest's entry point.
* `babel.config.js` is set up exactly as in Jest's docs [Getting started - Using TypeScript](https://jestjs.io/docs/getting-started#using-typescript)
* `jest.config.ts` is set up to mock any dependencies ending with `.svg` using `src/mock.js`, as in Jest's docs [Using with webpack - handling static assets](https://jestjs.io/docs/webpack#handling-static-assets)

Use `npm test` to run Jest.

### Expected behavior

There are no imported `.svg` dependencies, so nothing should get mocked.
The test should pass, because `foo()` returns `AlertOutlined` from `@ant-design/icons-svg` and the `expected` constant is identical to it.

### Actual behavior

The test fails with the error:

> TypeError: Cannot read property 'AlertOutlined' of undefined

Jest wrongly mocks `@ant-design/icons-svg` using `src/mock.js`, despite the dependency not ending with a `.svg`.
