<template>
  <div>
    <h2>已跳转到：news.index页</h2>
    <br />
    <ul>
      <li>
        <p>接收来自首页的参数: {{$route.params.testId}}</p>
        <p>接收来自首页的参数: {{$route.params.testName}}</p>
      </li>
    </ul>
    <br />
    <ul>
      <li>
        <nuxt-link :to="{name:'index'}">返回HOME</nuxt-link>
      </li>
    </ul>
    <br />
    <h3>用nuxt-link标签的写法</h3>
    <ul>
      <li>
        <nuxt-link :to="{name:'news-id',params:{id:123}}">跳转到动态路由页（/news/123）</nuxt-link>
      </li>
      <li>
        <nuxt-link :to="{name:'news-id',params:{id:'啊啊啊啊啊'}}">跳转到动态路由页（/news/啊啊啊啊）</nuxt-link>
      </li>
    </ul>

    <h3>用a标签的写法</h3>
    <ul>
      <li>
        <a href="/news/123">跳转到动态路由页（/news/123）</a>
      </li>
      <li>
        <a href="/news/啊啊啊啊">跳转到动态路由页（/news/啊啊啊啊）</a>
      </li>
    </ul>
  </div>
</template>