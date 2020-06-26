<template>
  <div>
    <h1>这是pages文件夹下面的index.vue首页</h1>
    <ul>
      <li>
        <nuxt-link :to="{name:'about',params:{testId:1234,testName:'示例文字'}}">跳转到about.index页</nuxt-link>
      </li>
      <li>
        <nuxt-link :to="{name:'news',params:{testId:1234,testName:'示例文字'}}">跳转到news.index页</nuxt-link>
      </li>
      <li>
        <nuxt-link :to="{name:'asyncData',params:{}}">跳转到asyncData异步加载数据页</nuxt-link>
      </li>
    </ul>
    <div class="imgLocation">
      <!-- <img src="../static/innocent.jpg" alt="无辜" style="max-width: 100px; max-height: 100px;" /> -->
    </div>
    <br />
    <div>
      <!-- <img src="~static/innocent.jpg" alt="无辜" style="max-width: 100px; max-height: 100px;" /> -->
    </div>
  </div>
</template>

<script>
export default {
  components: {}
};
</script>
<style>
.imgLocation {
  width: 100px;
  height: 100px;
  max-width: 100px;
  max-height: 100px;
  background-image: url("~static/innocent.jpg");
}
</style>