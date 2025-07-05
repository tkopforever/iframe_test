<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
import { ref } from 'vue'

const iframeRef = ref<HTMLIFrameElement | null>(null)

/**
 * 监听 iframe 加载完成事件
 * 监听页面中的消息事件，当接收到来自 iframe 的消息时，
 * 会检查消息的来源是否与当前页面的来源一致，如果一致则输出消息内容。
 */
const onIframeLoad = () => {
  window.addEventListener('message', (event) => {
    if (event.origin !== window.location.origin) return
    console.log('Message received from iframe:', event.data)
  })
}

/**
 * 向iframe发送消息
 *
 * 发送一个类型为 'test' 的消息到 iframe，消息内容为 'Hello from parent!'。
 * 如果 iframeRef.value?.contentWindow 存在，则发送消息；否则，不执行任何操作。
 */
const sendMessage = () => {
  const link = prompt('请输入要发送给iframe的HTTP链接', 'https://example.com')
  if (iframeRef.value?.contentWindow) {
    iframeRef.value.contentWindow.postMessage(
      {
        type: 'display-link',
        link: link || null
      },
      window.location.origin
    )
  }
}
</script>

<template>
  <div class="main-container">
    <div class="iframe-full-row">
      <button @click="sendMessage">发送message到iframe</button>
      <iframe ref="iframeRef" src="/iframe-test.html" @load="onIframeLoad"
        style="width: 100%; height: 400px; border: 1px solid #ccc;"></iframe>
    </div>
  </div>
</template>

<style scoped>
.main-container {
  width: 100%;
}

.iframe-full-row {
  margin-bottom: 20px;
}

.content-below {
  display: flex;
  align-items: center;
  gap: 20px;
  flex-wrap: wrap;
}

@media (max-width: 768px) {
  .content-below {
    flex-direction: column;
  }
}

.wrapper {
  flex: 1;
}

header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 2rem 0 0;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;
    padding: 1rem 0;
    margin-top: 1rem;
    width: 100%;
  }
}
</style>
