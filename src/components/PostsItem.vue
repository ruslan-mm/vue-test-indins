<template>
    <li>
        <div>
            <span>{{ new Date(obj.date).toLocaleString() }} / </span><span><b>{{ obj.authorName }}</b> / </span><a
                :href="obj.authorUrl">{{ obj.authorUrl }}</a>
        </div>
        <p v-html="obj.content"></p>
        <hr>
    </li>
</template>

<script>
export default {
    props: ['obj']
}
</script>

<style scoped>
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