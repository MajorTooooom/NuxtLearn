<template>
  <div>
    <h1>当前在异步请求数据页`pages\asyncData.vue`</h1>
    <br />
    <div>{{ name }}</div>
    <div>
      <h3>以下是从SpringBoot后台获取的数据</h3>
      <br />
      <p>姓名： {{info.name }}</p>
      <p>年龄： {{info.age }}</p>
      <p>爱好： {{info.love }}</p>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  components: {},
  data() {
    return {
      name: "Hello World!"
    };
  },
  //   2种方法之一：promise
  asyncData() {
    return axios.get("http://localhost:6688/data啊啊啊啊啊").then(res => {
      return { info: res.data };
    });
  },
  async asyncData() {
    let { data } = await axios.get("http://localhost:6688/data");//定义一个data变量，它的值是由axios.get()方法获取，await的意思是等待结果执行完毕才返回值；
    return { info: data };//将结果return，也是return到data()里面；
  }
};
</script>