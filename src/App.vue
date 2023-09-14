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
        <n-dynamic-input v-if="!isTranslate" @keyup="handleKeyUp" v-model:value="lyricList" placeholder="请输入" :min="1" />
      </n-layout-content>
    </n-layout>
  </n-space>
</template>

<script setup>

const lyricList = ref([""])

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

const isTranslate = ref(false)

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
