<template>
  <n-space vertical size="large">
    <n-layout position="absolute">
      <n-layout-header>
        <n-space justify="center" :align="'center'" :size="50">
          <n-button  @click="handleKeyUp" size="large">导入文本</n-button>
          <n-button size="large">导出文本</n-button>
          <n-switch size="large">
            <template #checked>
              翻译
            </template>
            <template #unchecked>
              歌词
            </template>
          </n-switch>
        </n-space>
      </n-layout-header>
      <n-layout-content style="padding-top: 5%;width: 70%;margin: auto" >
        <n-dynamic-input  @keyup="handleKeyUp"  v-model:value="lyricList" placeholder="请输入" :min="1"  />
      </n-layout-content>
      
    </n-layout>
  </n-space>
</template>

<script setup>


const lyricList = ref([""])

function handleKeyUp(value,index) {
  // if(value.code === 'Enter') {
  //   lyricList.value.push("");
  // }
}
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
