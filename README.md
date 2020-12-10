1
2
3
<h1 align="center">Welcome to rxv 👋</h1>
<p>
  <a href="https://www.npmjs.com/package/rxv" target="_blank">
    <img alt="Version" src="https://img.shields.io/npm/v/rxv.svg">
  </a>
  <a href="https://sl1673495.github.io/react-composition-api" target="_blank">
    <img alt="Documentation" src="https://img.shields.io/badge/documentation-yes-brightgreen.svg" />
  </a>
  <a href="#" target="_blank">
    <img alt="License: MIT" src="https://img.shields.io/badge/License-MIT-yellow.svg" />
  </a>
</p>

> Use Vue3 Composition Api in React to manage state, just import @vue/reactivity, rxv means React x Vue

### 🏠 [Homepage](https://github.com/sl1673495/react-composition-api)

### ✨ [Demo](https://sl1673495.github.io/react-composition-api)

# rxv

在 React 应用中使用@vue/reactivity 中的所有响应式能力

1. 使用 setup 在组件中体验 Vue-Composition-Api
2. 使用极简的 api 实现全局状态管理

## Docs

https://sl1673495.github.io/react-composition-api

## Why

1. 直接引入`@vue/reacivity`，完全使用 Vue3 的 reactivity 能力，拥有`computed`, `effect`等各种能力，并且对于`Set`和`Map`也提供了响应式的能力。
   后续也会随着这个库的更新变得更加完善的和强大。
2. `vue-next`仓库内部完整的测试用例。
3. 完善的 TypeScript 类型支持。
4. 完全复用`@vue/reacivity`实现超强的**全局状态管理能力**。
5. 状态管理中组件级别的精确更新。

## Usage

```
npm i rxv -S
npm i @vue/reactivity -S
```

支持[@vue/reactivity](https://www.npmjs.com/package/@vue/reactivity)内部提供的所有 api，并在 React 组件中使用。

### 快速上手示例

store：

```ts
import { reactive } from "@vue/reactivity"

export const state = reactive({
  count: 0,
})

export const add = () => (state.count += 1)

export const store = reactive(state)

export type Store = typeof store
```

组件：

```tsx
import React from "react"
import { Provider, useStore } from "rxv"
import { store, add, Store } from "./store"

function Count() {
  const countState = useStore((store: Store) => ({
    count: store.count,
  }))

  return (
    <Card hoverable style={{ marginBottom: 24 }}>
      <h1>计数器</h1>
      <div className="chunk">
        <div className="text-chunk">
          store中的count现在是 {countState.count}
        </div>
        <Button onClick={add}>add</Button>
      </div>
    </Card>
  )
}

export default () => {
  return (
    <Provider value={store}>
      <div className="flex">
        <Count />
      </div>
    </Provider>
  )
}
```

### 相对复杂的示例

这个例子里使用了 Vue3 中的 `computed`、`effect` 等能力，是一个相对比较复杂的示例。

```ts
// store.ts
import { reactive, computed, effect } from "@vue/reactivity"

export const state = reactive({
  count: 0,
})

const plusOne = computed(() => state.count + 1)

effect(() => {
  console.log("plusOne changed: ", plusOne)
})

const add = () => (state.count += 1)

export const mutations = {
  // mutation
  add,
}

export const store = {
  state,
  computed: {
    plusOne,
  },
}

export type Store = typeof store
```

```tsx
// Index.tsx
import { Provider, useStore } from "rxv"
function Count() {
  const countState = useStore((store: Store) => {
    const { state, computed } = store
    const { count } = state
    const { plusOne } = computed

    return {
      count,
      plusOne,
    }
  })

  return (
    <Card hoverable style={{ marginBottom: 24 }}>
      <h1>计数器</h1>
      <div className="chunk">
        <div className="chunk">store中的count现在是 {countState.count}</div>
        <div className="chunk">
          computed值中的plusOne现在是 {countState.plusOne.value}
        </div>
        <Button onClick={mutations.add}>add</Button>
      </div>
    </Card>
  )
}

export default () => {
  return (
    <Provider value={store}>
      <Count />
    </Provider>
  )
}
```

可以看出来，store 的定义完全复用了`@vue/reactivity`中的能力，而不会引入额外的学习成本，并且里面的所有能力都可以完美支持。

具体的代码和效果可以看[文档](https://sl1673495.github.io/react-composition-api)中的 `复杂示例`。

## 支持的 Vue3 api

除了内置的几个 api 以外，其他所有的 api 都是 Vue 内部提供的。

具体支持的 api，可以看`vue-next`仓库中的导出的 api

```ts
export { ref, isRef, toRefs, Ref, UnwrapRef } from "./ref"
export {
  reactive,
  isReactive,
  readonly,
  isReadonly,
  shallowReadonly,
  toRaw,
  markReadonly,
  markNonReactive,
} from "./reactive"
export {
  computed,
  ComputedRef,
  WritableComputedRef,
  WritableComputedOptions,
  ComputedGetter,
  ComputedSetter,
} from "./computed"
export {
  effect,
  stop,
  pauseTracking,
  resumeTracking,
  ITERATE_KEY,
  ReactiveEffect,
  ReactiveEffectOptions,
  DebuggerEvent,
} from "./effect"
export { lock, unlock } from "./lock"
export { TrackOpTypes, TriggerOpTypes } from "./operations"
```

注意`computed`、`ref`这些包装后的值没有提供自动拆包的功能，必须用`data.value`去读取和赋值。

## LICENSE

MIT

## Author

👤 **ssh**

- Website: https://blog.sl1673495.now.sh/
- Github: [@sl1673495](https://github.com/sl1673495)

## Show your support

Give a ⭐️ if this project helped you!

---

_This README was generated with ❤️ by [readme-md-generator](https://github.com/kefranabg/readme-md-generator)_
