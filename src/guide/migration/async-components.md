---
badges:
  - new
---

# Async Components <MigrationBadges :badges="$frontmatter.badges" />
# 非同期コンポーネント <MigrationBadges :badges="$frontmatter.badges" />

## 概要

Here is a high level overview of what has changed:
変更点の概要はこちらです:

- New `defineAsyncComponent` helper method that explicitly defines async components
- `component` option renamed to `loader`
- Loader function does not inherently receive `resolve` and `reject` arguments and must return a Promise

- 明示的に非同期コンポーネントを定義する `defineAsyncComponent` ヘルパーメソッドが導入されました。
- `component` オプションが `loader` に名前が変更されました。
- loader 関数が引数として、 `resolve` と `reject` を受け取らなくなり、Promise オブジェクトを必ず返す必要があります。

For a more in-depth explanation, read on!

## イントロダクション

Previously, async components were created by simply defining a component as a function that returned a promise, such as:
以前は、非同期コンポーネントは、単純にコンポーネントを Promise を返す関数として定義することで作ることができました。例えば:

```js
const asyncPage = () => import('./NextPage.vue')
```

Or, for the more advanced component syntax with options:
もしくは、オプションを使う場合:

```js
const asyncPage = {
  component: () => import('./NextPage.vue'),
  delay: 200,
  timeout: 3000,
  error: ErrorComponent,
  loading: LoadingComponent
}
```

## 3.x でのシンタックス

Now, in Vue 3, since functional components are defined as pure functions, async components definitions need to be explicitly defined by wrapping it in a new `defineAsyncComponent` helper:
Vue 3 では、関数コンポーネントは純粋な関数として定義されるため、非同期コンポーネントの定義は、新しく導入された `defineAsyncComponent` ヘルパーによって明示的に定義される必要があります。

```js
import { defineAsyncComponent } from 'vue'
import ErrorComponent from './components/ErrorComponent.vue'
import LoadingComponent from './components/LoadingComponent.vue'

// Async component without options
// オプション無しの非同期コンポーネント
const asyncPage = defineAsyncComponent(() => import('./NextPage.vue'))

// Async component with options
// オプションありの非同期コンポーネント
const asyncPageWithOptions = defineAsyncComponent({
  loader: () => import('./NextPage.vue'),
  delay: 200,
  timeout: 3000,
  errorComponent: ErrorComponent,
  loadingComponent: LoadingComponent
})
```

Another change that has been made from 2.x is that the `component` option is now renamed to `loader` in order to accurately communicate that a component definition cannot be provided directly.
他の変更としては、 `component` オプションが、 `loader` に名前が変更された点があります。

```js{4}
import { defineAsyncComponent } from 'vue'

const asyncPageWithOptions = defineAsyncComponent({
  loader: () => import('./NextPage.vue'),
  delay: 200,
  timeout: 3000,
  error: ErrorComponent,
  loading: LoadingComponent
})
```

In addition, unlike 2.x, the loader function no longer receives the `resolve` and `reject` arguments and must always return a Promise.
それに加えて、2.x とは異なり、`loader` 関数は `resolve` と `reject` の引数を受け取らなくなり、必ず Promise オブジェクトを返す必要があります。

```js
// 2.x version
const oldAsyncComponent = (resolve, reject) => {
  /* ... */
}

// 3.x version
const asyncComponent = defineAsyncComponent(
  () =>
    new Promise((resolve, reject) => {
      /* ... */
    })
)
```

For more information on the usage of async components, see:
より詳細な非同期コンポーネントについては、こちら:

- [Guide: Dynamic & Async Components](/guide/component-dynamic-async.html#dynamic-components-with-keep-alive)
- [動的＆非同期コンポーネント]()
