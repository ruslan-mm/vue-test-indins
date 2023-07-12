<template>
  <div id="app">
    <ul>
      <li v-for="(obj, i) in changedData" :key="i">
        <div v-html="obj.content"></div>
      </li>
    </ul>
  </div>
</template>

<script>
import json from '@/db/feed.json'

export default {
  data() {
    return {
      dataArray: json,
    }
  },
  computed: {
    changedData() {
      return this.dataArray.map(el => {
        const clearedString = el.content.replace(/\n/g, ' ')
        el.contentPostTones.forEach(strEl => {
          const findedStr = clearedString.substring(strEl.position, strEl.position + strEl.length)
          const startStr = clearedString.substring(0, strEl.position)
          const endStr = clearedString.substring(strEl.position + strEl.length)
          el.content = el.content.replace(findedStr, `<span style="background-color: rgba(${this.getActuallColor(strEl.tone)}, 0.2)">${findedStr}</span>`)
        })
        return el
      })
    },
  },
  methods: {
    getActuallColor(tone) {
      if (tone < 0) return `255, ${255 * Math.abs(tone)}, 0`
      if (tone > 0) return `${255 * Math.abs(tone)}, 255, 0`
      if (tone == 0) return `255, 255, 0`
    },

  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
ul {
  list-style: none;
}
li { 
  padding: 15px;
}
</style>
