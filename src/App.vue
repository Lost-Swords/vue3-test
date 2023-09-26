<template>
  <n-space vertical size="large">
    <n-layout position="absolute">
      <n-layout-header>
        <n-space justify="center" :align="'center'" :size="50">
          <n-button @click="handleKeyUp" size="large">导入文本</n-button>
          <n-button size="large">导出文本</n-button>
          <n-switch size="large" v-model:value="isTranslate">
            <template #checked>
              翻译
            </template>
            <template #unchecked>
              歌词
            </template>
          </n-switch>
        </n-space>
      </n-layout-header>
      <n-layout-content style="padding-top: 5%;width: 70%;margin: auto">
        <n-dynamic-input v-if="!isTranslate" @keyup="handleKeyUp" v-model:value="lyricList" placeholder="请输入歌词"
          :min="1" />
        <n-dynamic-input v-else v-model:value="translateList" placeholder="请输入翻译" :min="1"  #="{ index, value }" >
            <div style="display: flex; align-items: center; width: 100%">
              <n-input v-model:value="lyricList[index]" disable type="text" />
              <n-input v-model:value="translateList[index]" type="text" />
            </div>
        </n-dynamic-input>
      </n-layout-content>
    </n-layout>
  </n-space>
</template>

<script setup>

const lyricList = ref([""])
const translateList = ref([""])
const isTranslate = ref(false)

async function handleKeyUp(value) {
  if (value.code === 'Enter') {
    if (findRootNodeWithElement(value.target, 'data-key') === (lyricList.value.length - 1).toString()) {
      lyricList.value.push("");
      await nextTick();
    }
    let inputs = document.getElementsByTagName("input");
    for (var i = 0; i < inputs.length; i++) {
      // 如果是最后一个，则焦点回到第一个
      if (value.srcElement == inputs[i]) {
        inputs[i + 1].focus(); break;
      }
    }
  }
}
function findRootNodeWithElement(element, targetTagName) {
  // 从当前元素开始向上遍历DOM树
  if (element !== null && element.getAttribute(targetTagName)) {
    return element.getAttribute(targetTagName);
  } else if (element.parentElement) {
    // 如果找到包含目标元素的根节点，则返回该根节点；否则返回null
    return findRootNodeWithElement(element.parentElement, targetTagName);
  }
  return null
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
