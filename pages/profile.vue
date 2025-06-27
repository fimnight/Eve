<template>
  <v-container fluid class="pa-0">
    <v-card flat tile>
      <v-img
        src="https://via.placeholder.com/1500x300?text=Your+Banner+Image" height="200px"
        cover
      ></v-img>

      <v-row class="ma-0 pa-0">
        <v-col cols="12" class="d-flex justify-space-between align-end pa-4">
          <v-avatar
            size="120"
            class="profile-avatar"
          >
            <img
              :src="userPhotoURL || 'https://via.placeholder.com/120'" alt="Profile Picture"
            >
          </v-avatar>

          <v-btn
            outlined
            rounded
            color="primary"
            class="text-none font-weight-bold"
            @click="editProfile"
          >
            แก้ไขข้อมูลส่วนตัว
          </v-btn>
        </v-col>
      </v-row>

      <v-card-text class="pt-0">
        <h2 class="text-h5 font-weight-bold mb-1">{{ userName || userEmail || 'ผู้ใช้' }}</h2>
        <p class="text-subtitle-1 grey--text text--darken-1 mb-2">@{{ username || 'ยังไม่มีชื่อผู้ใช้' }}</p>

        <p v-if="userBio" class="text-body-1">{{ userBio }}</p>
        <p v-else class="text-body-1 grey--text text--darken-1">ยังไม่มีคำอธิบายโปรไฟล์</p>

        <div class="d-flex align-center text-body-2 grey--text text--darken-1 mt-2">
          <v-icon small class="mr-1">mdi-calendar</v-icon>
          <span>เข้าร่วมเมื่อ: {{ joinDate }}</span>
        </div>
      </v-card-text>

      <v-divider></v-divider>

      <v-tabs v-model="tab" background-color="white" color="primary" grow>
        <v-tab v-for="item in tabs" :key="item.id">
          {{ item.text }}
        </v-tab>
      </v-tabs>

      <v-tabs-items v-model="tab">
        <v-tab-item v-for="item in tabs" :key="item.id">
          <v-card flat>
            <v-card-text class="text-center py-5">
              <p>เนื้อหาสำหรับแท็บ: <strong>{{ item.text }}</strong></p>
              <div v-if="item.id === 0">
                <p>แสดงโพสต์ทั้งหมด</p>
              </div>
              <div v-else-if="item.id === 1">
                <p>แสดงการตอบกลับ</p>
              </div>
              <div v-else-if="item.id === 2">
                <p>แสดงรูปภาพ/วิดีโอ</p>
              </div>
              <div v-else-if="item.id === 3">
                <p>แสดงรายการที่ถูกใจ</p>
              </div>
            </v-card-text>
          </v-card>
        </v-tab-item>
      </v-tabs-items>

    </v-card>
  </v-container>
</template>

<script>
export default {
  name: 'ProfilePage',
  // middleware: 'auth', // สามารถเปิดใช้งานเมื่อ Firebase Auth ตั้งค่าสมบูรณ์และต้องการให้หน้านี้ป้องกันการเข้าถึง

  data() {
    return {
      user: null,
      userEmail: '',
      userName: '',
      username: '',
      userPhotoURL: '',
      userBio: 'Hello, this is my profile bio! I love coding with Nuxt.js and Vuetify.',
      joinDate: 'มิถุนายน 2568',
      tab: null,
      tabs: [
        { id: 0, text: 'โพสต์' },
        { id: 1, text: 'การตอบกลับ' },
        { id: 2, text: 'สื่อ' },
        { id: 3, text: 'ถูกใจ' },
      ],
    };
  },
  mounted() {
    this.$fire.auth.onAuthStateChanged(user => {
      if (user) {
        this.user = user;
        this.userEmail = user.email;
        this.userName = user.displayName || 'ผู้ใช้';
        this.userPhotoURL = user.photoURL || 'https://via.placeholder.com/120';
        this.username = user.uid.substring(0, 8);
      } else {
        this.user = null;
        this.userEmail = '';
        this.userName = '';
        this.username = '';
        this.userPhotoURL = 'https://via.placeholder.com/120';
      }
    });
  },
  methods: {
    editProfile() {
      alert('คุณกดปุ่มแก้ไขข้อมูลส่วนตัวแล้ว!');
      // คุณสามารถนำไปหน้า Form สำหรับแก้ไขโปรไฟล์ได้ที่นี่
      // this.$router.push('/settings/profile');
    }
  }
}
</script>

<style scoped>
.profile-avatar {
  border: 4px solid white; /* เพิ่มขอบสีขาวรอบรูปโปรไฟล์ */
  margin-top: -60px; /* ดันรูปโปรไฟล์ขึ้นไปทับ Banner เล็กน้อย */
  z-index: 1; /* ทำให้รูปโปรไฟล์อยู่ด้านบน */
  margin-left: 16px; /* ระยะห่างจากขอบซ้าย */
}

/* คุณสามารถปรับแต่งเพิ่มเติมได้ที่นี่ */
</style>