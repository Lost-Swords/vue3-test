<template>
  <n-space vertical size="large">
    <n-layout position="absolute">
      <n-layout-header>
        <n-space justify="center" :align="'center'" :size="50">
          <n-upload
            ref="upload"
            :default-upload="false"
            accept=".txt"
            @change="textUpload"
            :show-file-list="false"
            v-model:file-list="fileList"
          >
            <n-button>导入文本</n-button>
          </n-upload>
          <n-button @click="textExport" size="large">导出文本</n-button>
          <n-switch size="large" v-model:value="isTranslate">
            <template #checked> 翻译 </template>
            <template #unchecked> 歌词 </template>
          </n-switch>
        </n-space>
      </n-layout-header>
      <n-layout-content style="padding-top: 5%; width: 70%; margin: auto">
        <n-dynamic-input
          v-if="!isTranslate"
          @keyup="lyricListAdd"
          v-model:value="lyricList"
          placeholder="请输入歌词"
          :min="1"
        />
        <n-dynamic-input v-else v-model:value="lyricList" :min="1">
          <template #default="{ index, value }">
            <div style="display: flex; align-items: center; width: 100%">
              <p :text="lyricList[index]" style="width: 100%">
                {{ lyricList[index] ? lyricList[index] : '该行无文本' }}
              </p>
              <n-input
                v-model:value="translateList[index]"
                @keyup="focusNextInput"
                placeholder="请输入翻译"
                type="text"
              />
            </div>
          </template>
        </n-dynamic-input>
      </n-layout-content>
    </n-layout>
  </n-space>
</template>

<script setup>
import { getCurrentInstance } from 'vue'
import { textDownload, findRootNodeWithElement } from './utils/utils.js'
const lyricList = ref([''])
const translateList = ref([''])
const isTranslate = ref(false)
const fileList  = ref([]);

//递增歌词数组，并聚焦到下一个
async function lyricListAdd(value) {
  if (value.code !== 'Enter') {
    return
  }
  if (
    findRootNodeWithElement(value.target, 'data-key') ===
    (lyricList.value.length - 1).toString()
  ) {
    lyricList.value.push('')
    await nextTick()
  }
  let inputs = document.getElementsByTagName('input')
  for (var i = 0; i < inputs.length; i++) {
    if (value.srcElement == inputs[i]) {
      inputs[i + 1].focus()
      break
    }
  }
}

function textUpload(options) {
  let selectedFile = options.file.file
  if (selectedFile) {
    let reader = new FileReader()

    reader.onload = function (e) {
      let fileContent = e.target.result; // 文件内容
      let stringList = fileContent.split('\r\n');
      lyricList.value = stringList;
      fileList.value = [];
    }
    reader.readAsText(selectedFile)
  }

}

function focusNextInput(value) {
  if (value.code !== 'Enter') {
    return
  }
  let inputs = document.getElementsByTagName('input')
  for (var i = 0; i < inputs.length; i++) {
    if (value.srcElement == inputs[i]) {
      inputs[i + 1].focus()
      break
    }
  }
}

//导出歌词文本
function textExport() {
  if (lyricList.value.length <= 0) {
    return
  }
  let text = lyricList.value.reduce(
    (text, current, index) =>
      text + '\n' + current + ' ' + translateList.value[index],
    lyricList.value[0] + ' ' + translateList.value[0]
  )
  textDownload('lyric', text)
}

watch(
  () => lyricList.value,
  (value, prevCount) => {
    /* ... */
  }
)
</script>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
