<script setup lang="ts">
import { ref } from 'vue';
import VueMarkdownEditor, { xss } from '@kangc/v-md-editor';

let text = ref(`
#### 1.通过命令行方式启动
\`\`\`shell
./webServer -h
提示:更多指令通过 -help 获取
Usage of webServer:
  -clear
        清除缓存
  -d int
        是否后台运行，默认：0 [0/前台运行 1/后台运行 2/关闭后台运行]
  -help string
        帮助
  -i    是否生成配置文件
  -p int
        启动端口，默认：80  (default 80)
  -w string
        需要展示的web目录，默认：. [默认会使用当前路径下的index.html作为首页] (default ".")
\`\`\`
> #### 通过命令方式启动

\`\`\`shell
# 设置服务端口\`88\` web目录为\`web\`
./webServer -p 88 -w web
\`\`\`
`)
const editor = ref(null);
const toHtml = () => {
  // const html = xss.process(VueMarkdownEditor.vMdParser.themeConfig.markdownParser.render(text.value));
  // console.log(html)
  const element = document.getElementsByClassName(`vuepress-markdown-body`)[0]
  function getElementStyles(element: any, excludes = [`width`, `height`]) {
    const styles = window.getComputedStyle(element, null)
    return Object.entries(styles)
      .filter(
        ([key]) => styles.getPropertyValue(key) && !excludes.includes(key)
      )
      .map(([key, value]) => `${key}:${value};`)
      .join(``)
  }
  const setStyles = (element: any) => {
    element.setAttribute(`style`, getElementStyles(element))
    if (element.children.length) {
      Array.from(element.children).forEach((child) => setStyles(child))
    }
  }
  setStyles(element)
  console.log(element)
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
