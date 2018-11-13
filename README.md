# vue-monaco-ts

> Based of [egoist/vue-monaco](https://github.com/egoist/vue-monaco) and [leo-buneev/vue-monaco-cdn](https://github.com/leo-buneev/vue-monaco-cdn)

[Monaco Editor](https://github.com/Microsoft/monaco-editor) is the code editor that powers VS Code. This project aims to provide powerful and type-safe Vue.js component.

This package using monaco-editor bundling via webpack.
*   webpack processing can lead to [issues](https://github.com/Microsoft/monaco-editor-webpack-plugin/issues/17)
*   it can significantly [increase](https://github.com/Microsoft/monaco-editor-webpack-plugin/issues/40) webpack bundle size and build time.

## Install

```sh
yarn add vue-monaco-ts
# or
npm install vue-monaco-ts
```

## Usage

```vue
<template>
  <monaco-editor
    class="editor"
    v-model="code"
    language="javascript"
  />
  </monaco-editor>
</template>

<script>
import MonacoEditor from 'vue-monaco-cdn'

export default {
  components: {
    MonacoEditor
  },

  data() {
    return {
      code: 'const noop = () => {}'
    }
  }
}
</script>

<style>
.editor {
  width: 600px;
  height: 400px;
}
</style>
```

### Props

-   `value` - code
-   `language` - programming language that code will be in. [List of supported languages](https://github.com/Microsoft/monaco-languages)
-   `theme` - visual theme for editor
-   `options` - [monaco editor additional options](https://microsoft.github.io/monaco-editor/api/interfaces/monaco.editor.ieditorconstructionoptions.html)

### Methods

-   `getMonaco(): IStandaloneCodeEditor`: Return the [editor instance](https://microsoft.github.io/monaco-editor/api/interfaces/monaco.editor.istandalonecodeeditor.html).

Use `ref` to interact with the `MonacoEditor` component in order to access methods: `<MonacoEditor ref="editor"></MonacoEditor>`, then `this.$refs.editor.getMonaco()` will be available.

### Events

-   `editorDidMount` - fired after monaco editor was mounted. Recieves monaco instance (`IStandaloneCodeEditor`) as parameter. Use this event to customize monaco instance (for example, add new code formatters)

For other events, please use `getMonaco()` and subscribe to them directly. See [IStandaloneCodeEditor reference](https://microsoft.github.io/monaco-editor/api/interfaces/monaco.editor.istandalonecodeeditor.html) for full events list.

## Author

**vue-monaco-ts**. Released under the [MIT](./LICENSE) License.

Authored and maintained by [James Bruno](https://github.com/meta-exception/vue-monaco-ts) and contributors.
Huge thanks to [egoist](https://github.com/egoist/) and [leo-buneev](https://github.com/leo-buneev/).
