<template>
  <n-space  vertical size="large">
    <n-layout  position="absolute">
      <n-layout-header>
        <n-space justify="center" :size="50">
          <n-button size="large">导入文本</n-button>
          <n-button size="large">导出文本</n-button>
        </n-space>
      </n-layout-header>
      <n-layout-content style="padding-top: 5%;width: 70%;margin: auto" >
        <n-dynamic-input   v-model:value="value" placeholder="请输入" :min="3" :max="6" />
        <pre>{{ JSON.stringify(value, null, 2) }}</pre>
      </n-layout-content>
    </n-layout>
  </n-space>
</template>

<script setup>


const value = ref(["", "", ""])

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
