<template>
  <div>
    <q-select outlined v-model="type" :options="['less','scss']" :label="`${type} to css`" />
    <q-splitter :value="50" class="relative">
      <template v-slot:before>
        <div class="q-pa-md">
          <div class="text-h4 q-mb-md">{{type}}</div>
          <textarea name="less" id="source" v-model="style"></textarea>
        </div>
      </template>
      <q-btn round color="primary" icon="arrow_forward" class="confirm-btn" @click="convert" />
      <template v-slot:after>
        <div class="q-pa-md">
          <div class="text-h4 q-mb-md">css</div>

          <pre class="language-js"><code v-text="convertCss"></code></pre>
        </div>
      </template>
    </q-splitter>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Watch } from 'vue-property-decorator';
import Prism from 'prismjs';
import 'prismjs/themes/prism.css';
import 'codemirror/lib/codemirror.css';
import '@/utils/cssConvert';
import CodeMirror from 'codeMirror';
const lessStr = `@width:500px;@height:300px;@font_size:12px;textarea {width:@width;height:@height;font-size:@font_size;}`;
const scssStr = '';
@Component
export default class extends Vue {
  private type = 'less';
  private style = lessStr;
  private editor: any;
  private convertCss = '';
  private convert() {
    this.type === 'less' &&
      less.render(this.editor.getValue(), (e: any, data: any) => {
        this.convertCss = data.css;
        this.$nextTick(() => {
          Prism.highlightAll();
        });
      });
  }
  private init() {
    this.$nextTick(() => {
      const $source: any = document.getElementById('source');
      this.editor = CodeMirror.fromTextArea($source, {
        mode: 'css',
        lineNumbers: true,
        lineWrapping: true,
      });
      less.render(this.editor.getValue(), (e: any, data: any) => {
        this.convertCss = data.css;
        this.$nextTick(() => {
          Prism.highlightAll();
        });
      });
    });
  }
  mounted() {
    this.init();
  }
}
</script>

<style scoped lang='scss'>
.confirm-btn {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  top: 20%;
  z-index: 1000;
}
</style>
