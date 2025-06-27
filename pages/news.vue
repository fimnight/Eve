<template>
  <v-container>
    <v-row justify="center" align="center">
      <v-col cols="12">
        <v-card class="elevation-6">
          <v-card-title class="headline">
            <v-icon class="mr-2">mdi-newspaper-variant-multiple-outline</v-icon>
            ข่าวสาร
          </v-card-title>
          <v-card-text>
            <p v-if="loading">กำลังโหลดข่าวสาร...</p>
            <p v-if="error">{{ error }}</p>

            <v-list two-line>
              <v-list-item
                v-for="article in articles"
                :key="article.url"
                @click="openArticle(article.url)"
                class="mb-4"
              >
                <v-list-item-avatar size="80" rounded>
                  <v-img :src="article.urlToImage || 'https://via.placeholder.com/80?text=No+Image'"></v-img>
                </v-list-item-avatar>
                <v-list-item-content>
                  <v-list-item-title class="font-weight-bold">{{ article.title }}</v-list-item-title>
                  <v-list-item-subtitle class="text-caption grey--text text--darken-1">
                    {{ article.source.name }} - {{ formatDate(article.publishedAt) }}
                  </v-list-item-subtitle>
                  <v-list-item-subtitle>{{ article.description }}</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </v-list>

            <div v-if="articles.length === 0 && !loading && !error" class="text-center">
              <p>ไม่พบข่าวสารในขณะนี้</p>
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'NewsPage',
  data() {
    return {
      articles: [],
      loading: true,
      error: null,
      newsApiKey: '21efa29cbb3c4d6a9f773e736e0994c0', // <--- ***** เปลี่ยนตรงนี้ด้วย API Key ของคุณ *****
    };
  },
  async fetch() {
    try {
      const response = await this.$axios.$get(
        `https://newsapi.org/v2/top-headlines?sources=bbc-news&apiKey=${this.newsApiKey}` // ดึงข่าวจาก BBC News
        // `https://newsapi.org/v2/top-headlines?country=us&apiKey=${this.newsApiKey}` // หรือดึงข่าวจาก US
      );
      this.articles = response.articles;
    } catch (e) {
      console.error('Error fetching news:', e);
      this.error = 'ไม่สามารถดึงข่าวสารได้ในขณะนี้ กรุณาลองใหม่อีกครั้ง';
    } finally {
      this.loading = false;
    }
  },
  methods: {
    formatDate(dateString) {
      const options = { year: 'numeric', month: 'long', day: 'numeric', hour: '2-digit', minute: '2-digit' };
      return new Date(dateString).toLocaleDateString('th-TH', options);
    },
    openArticle(url) {
      window.open(url, '_blank'); // เปิดลิงก์ข่าวในแท็บใหม่
    }
  }
}
</script>

<style scoped>
/* คุณสามารถเพิ่ม Style เฉพาะหน้านี้ได้ที่นี่ */
</style>