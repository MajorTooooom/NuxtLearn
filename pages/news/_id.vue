<template>
  <div>
    <h1>正位于news文件夹下的_id.vue页面</h1>
    <nuxt-link :to="{name:'index'}">返回首页</nuxt-link>
    <br />
    <p>接收news/index.vue页面传递的参数： {{$route.params.id}}</p>
  </div>
</template>
<script>
export default {
  validate({ params }) {
    // 必须是number类型
    return /^\d+$/.test(params.id); //这里/^\d+$/.test(params.id)的返回值是true/false
    // 如果return true，就会正常进入页面，如果false则跳错误页面
  },
  data() {
    return {
      title: this.$route.params.title
    };
  },
  head() {
    return {
      title: this.title,
      meta: [
        {
          hid: "自定义meta",
          name: "news1",
          content: "这是一个news标签"
        }
      ]
    };
  }
};
</script>