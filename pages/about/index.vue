<template>
  <div>
    <h2>已跳转到：about.index页</h2>
    <ul>
      <li>
        <nuxt-link :to="{name:'index'}">返回HOME</nuxt-link>
      </li>
      <li>
        <p>从后端服务器(IDEA SpringBoot Web)接收参数: {{ user.name }}</p>
      </li>
      <li>
        <p>接收来自首页的参数: {{$route.params.testId}}</p>
        <p>接收来自首页的参数: {{$route.params.testName}}</p>
      </li>
    </ul>
  </div>
</template>
<script>
import axios from "axios";
export default {
  async asyncData() {
    const { data } = await axios.get("http://192.168.1.105:6688/data");
    return { user: data };
  }
};
</script>