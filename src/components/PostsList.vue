<template>
    <div>
        <ul>
            <PostsItem v-for="(obj, i) in cuttedData" :key="i" :obj="obj" />
        </ul>
        <button v-if="this.changedData.length - this.changedData.slice(0, this.postsPerPage).length"
            @click="addPosts">Показать еще {{ this.counterPosts }} стихов</button>
        <div class="load" v-if="isLoad"><img src="../../public/img/1487.gif" alt=""></div>
    </div>
</template>

<script>
import PostsItem from '@/components/PostsItem.vue';
import json from '@/db/feed.json'

export default {
    data() {
        return {
            dataArray: json,
            postsPerPage: 5,
            isLoad: false
        }
    },
    components: {
        PostsItem
    },
    computed: {
        changedData() {
            return this.dataArray.map(el => {
                let clearedString = el.content.replace("\n", " ").replace(/  +/g, ' ')
                let resultString = ""
                let lastIndex = 0
                el.contentPostTones.forEach(strEl => {
                    resultString += clearedString.substring(lastIndex, strEl.position)
                    resultString += `<span style="background-color: rgba(${this.getActuallColor(strEl.tone)}, 0.2)">${clearedString.substring(strEl.position, strEl.position + strEl.length)}</span>`
                    lastIndex = strEl.position + strEl.length
                })
                resultString += clearedString.substring(lastIndex)

                el.content = resultString
                return el
            })
        },
        cuttedData() {
            return this.changedData.slice(0, this.postsPerPage)
        },
        counterPosts() {
            if ((this.changedData.length - this.changedData.slice(0, this.postsPerPage).length) < 10) {
                return this.changedData.length - this.changedData.slice(0, this.postsPerPage).length
            } else {
                return 10
            }
        }
    },
    methods: {
        addPosts() {
            this.isLoad = true
            return setTimeout(() => {
                this.postsPerPage += 10
                this.isLoad = false
            }, 1000)
        },
        getActuallColor(tone) {
            if (tone < 0) return `255, ${255 * Math.abs(tone)}, 0`
            if (tone > 0) return `${255 * Math.abs(tone)}, 255, 0`
            if (tone == 0) return `255, 255, 0`
        },

    }
}
</script>
    
<style scoped>
ul {
    margin: 0;
    padding: 0;
    list-style: none;
}

button {
    margin-left: 10px;
}

.load {
    position: fixed;
    top: 30%;
    left: 45%;
}
</style>