<template>
  <section class="container">
    <div>
      <logo/>
      <h1 class="title">
        Rss Reader
      </h1>
      <h2 class="subtitle">
        Feed
      </h2>
      <ul>
        <li 
          v-for="item in data" 
          :key="item.title">
          <a :href="item.link">
            {{ item.title }}
          </a>
        </li>
      </ul>
      <!-- <div class="links">
        <a
          href="https://nuxtjs.org/"
          target="_blank"
          class="button--green">Documentation</a>
        <a
          href="https://github.com/nuxt/nuxt.js"
          target="_blank"
          class="button--grey">GitHub</a>
      </div> -->
    </div>
  </section>
</template>

<script>
// import Logo from '~/components/Logo.vue'
// export default {
//   components: {
//     Logo
//   }
// }
import axios from 'axios'
export default {
  asyncData({ params }, callback) {
    axios
      .get(`http://rssblog.ameba.jp/mourin-24/rss20.xml`)
      .then(res => {
        var parseString = require('xml2js').parseString
        var xml = res.data
        parseString(xml, (message, xmlres) => {
          callback(null, { data: xmlres.rss.channel[0].item })
        })
      })
      .catch(e => {
        callback({ statusCode: 404, message: 'ページが見つかりません' })
      })
  }
}
</script>

<style>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
