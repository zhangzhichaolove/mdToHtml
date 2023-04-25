<script setup lang="ts">
import { ref } from 'vue';
import VueMarkdownEditor, { xss } from '@kangc/v-md-editor';
let text = ref('# 首页')
const editor = ref(null);
const toHtml = () => {
  const html = xss.process(VueMarkdownEditor.vMdParser.themeConfig.markdownParser.render(text.value));
  console.log(html)
}
</script>

<template>
  <div>
    <button @click="toHtml">输出HTML</button>
  </div>
  <v-md-editor ref="editor" v-model="text"></v-md-editor>
</template>

<style scoped>
.v-md-editor {
  width: 100%;
  height: 100%;
}
</style>
