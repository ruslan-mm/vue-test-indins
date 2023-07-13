<template>
  <div id="app">
    <ul>
      <li v-for="(obj, i) in changedData" :key="i">
        <div>
          <span>{{ obj.date }} / </span><span><b>{{ obj.authorName }}</b> / </span><a :href="obj.authorUrl">{{ obj.authorUrl }}</a>
        </div>
        <p v-html="obj.content"></p>
        <hr>
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
          let clearedString = el.content.replace("\n", " ").replace( /  +/g, ' ' )
          let resultString = ""
          let lastIndex = 0
          el.contentPostTones.forEach(strEl => {
            resultString += clearedString.substring(lastIndex, strEl.position)
            resultString += `<span style="background-color: rgba(${this.getActuallColor(strEl.tone)}, 0.2)">${clearedString.substring(strEl.position, strEl.position + strEl.length)}</span>`
            lastIndex = strEl.position + strEl.length
            console.log(resultString);
          })
          resultString += clearedString.substring(lastIndex)
  
          el.content = resultString
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
  color: #2c3e50;
  margin-top: 60px;
}
ul {
  margin: 0;
  padding: 0;
  list-style: none;
}
li {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin: 0; 
  padding: 15px;
}
hr {
  width: 100%;
}
a {
  text-decoration: none;
  color: inherit;
}
</style>
