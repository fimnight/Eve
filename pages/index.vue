<template>
  <div>
    <h1>หน้าแรก (Home)</h1>
    <ul>
      <li v-for="post in posts" :key="post.id">
        <h3>{{ post.title }}</h3>
        <p>{{ post.body }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    try {
      const res = await $axios.get('https://jsonplaceholder.typicode.com/posts')
      return { posts: res.data }
    } catch (error) {
      console.error('ดึงข้อมูลไม่สำเร็จ:', error)
      return { posts: [] }
    }
  }
}
</script>
