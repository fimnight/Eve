<template>
  <v-container>
    <v-row justify="center" align="center">
      <v-col cols="12" sm="8" md="6">
        <v-card class="elevation-6">
          <v-card-title class="headline">
            <v-icon class="mr-2">mdi-magnify</v-icon>
            สำรวจ (Google Search)
          </v-card-title>
          <v-card-text>
            <v-text-field
              v-model="searchQuery"
              label="ค้นหาใน Google"
              solo
              clearable
              append-icon="mdi-magnify"
              @click:append="searchGoogle"
              @keyup.enter="searchGoogle"
            ></v-text-field>

            <v-btn
              color="primary"
              block
              @click="searchGoogle"
              :disabled="!searchQuery"
            >
              ค้นหา
            </v-btn>

            <p class="mt-4 text-center grey--text">
              ป้อนคำค้นหาของคุณเพื่อค้นหาข้อมูลบน Google.com
            </p>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'ExplorePage',
  data() {
    return {
      searchQuery: '', // เก็บข้อความที่ผู้ใช้ป้อน
    };
  },
  methods: {
    searchGoogle() {
      if (this.searchQuery) {
        // เข้ารหัสคำค้นหาเพื่อให้ปลอดภัยสำหรับ URL
        const encodedQuery = encodeURIComponent(this.searchQuery);
        // สร้าง URL สำหรับ Google Search
        const googleSearchUrl = `https://www.google.com/search?q=${encodedQuery}`;
        // เปิด URL ในแท็บใหม่
        window.open(googleSearchUrl, '_blank');
      } else {
        alert('กรุณาป้อนคำค้นหา');
      }
    }
  }
}
</script>

<style scoped>
/* คุณสามารถเพิ่ม Style เฉพาะหน้านี้ได้ที่นี่ */
</style>