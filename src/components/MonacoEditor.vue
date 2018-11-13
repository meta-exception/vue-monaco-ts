<template>
  <div class="monaco-editor">
  </div>
</template>

<script lang="ts">
// monaco-editor is in esm format, so cannot be resolved
// eslint-disable-next-line import/no-unresolved
import * as monaco from 'monaco-editor';
import Vue from 'vue';

export default Vue.extend({
  name: 'MonacoEditor',

  props: {
    value: {
      type: String,
      default: '',
    },
    language: {
      type: String,
      default: 'markdown',
    },
    theme: {
      type: String,
      default: 'vs',
    },
    options: {
      type: Object,
      default: null,
    },
  },

  data(): {
    editor: monaco.editor.IStandaloneCodeEditor,
  } {
    return {
      editor: (this as any).editor,
    };
  },

  model: {
    event: 'change',
  },

  watch: {
    options: {
      deep: true,
      handler(options: object) {
        if (this.editor) {
          this.editor.updateOptions(options);
        }
      },
    },

    value(newValue: string) {
      if (this.editor) {
        if (newValue !== this.editor.getValue()) {
          this.editor.setValue(newValue);
        }
      }
    },

    language(newLang: string) {
      if (this.editor) {
        monaco.editor.setModelLanguage(this.editor.getModel(), newLang);
      }
    },

    theme(newTheme: string) {
      if (this.editor) {
        monaco.editor.setTheme(newTheme);
      }
    },
  },

  mounted() {
    this.init();
  },

  beforeDestroy() {
    if (this.editor) {
      this.editor.dispose();
    }
  },

  methods: {
    // tslint:disable-next-line:no-shadowed-variable
    init() {
      const options = {
        value: this.value,
        theme: this.theme,
        language: this.language,
        ...this.options,
      };

      this.editor = monaco.editor.create(this.$el, options);
      console.log('spawn', options);

      this.$emit('editorDidMount', this.editor);

      this.editor.onContextMenu((event: monaco.editor.IEditorMouseEvent) => {
        this.$emit('contextMenu', event);
      });

      this.editor.onDidBlurEditorText(() => this.$emit('blurText'));

      this.editor.onDidBlurEditorWidget(() => this.$emit('blurWiget'));

      this.editor.onDidChangeConfiguration((event: monaco.editor.IConfigurationChangedEvent) => {
        this.$emit('configuration', event);
      });

      this.editor.onDidChangeCursorPosition((event: monaco.editor.ICursorPositionChangedEvent) => {
        this.$emit('position', event);
      });

      this.editor.onDidChangeCursorSelection((event: monaco.editor.ICursorSelectionChangedEvent) => {
        this.$emit('selection', event);
      });

      this.editor.onDidChangeModel((event: monaco.editor.IModelChangedEvent) => {
        this.$emit('model', event);
      });

      this.editor.onDidChangeModelContent((event: monaco.editor.IModelContentChangedEvent) => {
        const value = this.editor.getValue();
        if (this.value !== value) {
          this.$emit('change', value, event);
        }
      });

      this.editor.onDidChangeModelDecorations((event: monaco.editor.IModelDecorationsChangedEvent) => {
        this.$emit('modelDecorations', event);
      });

      this.editor.onDidChangeModelLanguage((event: monaco.editor.IModelLanguageChangedEvent) => {
        this.$emit('modelLanguage', event);
      });

      this.editor.onDidChangeModelOptions((event: monaco.editor.IModelOptionsChangedEvent) => {
        this.$emit('modelOptions', event);
      });

      this.editor.onDidDispose(() => this.$emit('afterDispose'));

      this.editor.onDidFocusEditorText(() => this.$emit('focusText'));

      this.editor.onDidFocusEditorWidget(() => this.$emit('focusWidget'));

      this.editor.onDidLayoutChange((event: monaco.editor.EditorLayoutInfo) => {
        this.$emit('layout', event);
      });

      this.editor.onDidScrollChange((event: monaco.IScrollEvent) => {
        this.$emit('scroll', event);
      });

      this.editor.onKeyDown((event: monaco.IKeyboardEvent) => {
        this.$emit('keydown', event);
      });

      this.editor.onKeyUp((event: monaco.IKeyboardEvent) => {
        this.$emit('keyup', event);
      });

      this.editor.onMouseDown((event: monaco.editor.IEditorMouseEvent) => {
        this.$emit('mouseDown', event);
      });

      this.editor.onMouseUp((event: monaco.editor.IEditorMouseEvent) => {
        this.$emit('mouseUp', event);
      });

      this.editor.onMouseMove((event: monaco.editor.IEditorMouseEvent) => {
        this.$emit('mouseMove', event);
      });

      this.editor.onMouseLeave((event: monaco.editor.IEditorMouseEvent) => {
        this.$emit('mouseLeave', event);
      });
    },

    getMonaco() {
      return (this as any).editor;
    },

    focus() {
      this.editor.focus();
    },
  },
});
</script>
