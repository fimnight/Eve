<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant.sync="miniVariant"  :clipped="clipped"
      fixed
      app
      permanent                  expand-on-hover            :width="260"               mini-variant-width="72"    color="white"             light                      class="border-right"       >
      <v-list-item class="px-2 mt-2">
        <v-list-item-avatar>
          <v-img src="/x-logo.png"></v-img> </v-list-item-avatar>
        <v-list-item-title class="font-weight-bold" :class="{'hidden-sm-and-down': miniVariant}">
          Your App Name
        </v-list-item-title>
      </v-list-item>
      <v-divider></v-divider>

      <v-list dense nav>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          nuxt
          router
          exact
        >
          <v-list-item-action>
            <v-icon size="28">{{ item.icon }}</v-icon> </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" class="font-weight-bold" />
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <v-spacer></v-spacer>

      <v-list-item class="mb-2">
        <v-menu offset-y top> <template v-slot:activator="{ on, attrs }">
            <v-list-item-avatar
              size="40"
              v-bind="attrs"
              v-on="on"
            >
              <img
                :src="userPhotoURL || 'https://via.placeholder.com/40'"
                alt="User Avatar"
              >
            </v-list-item-avatar>
          </template>
          <v-list>
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title class="font-weight-bold">{{ userEmail }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item @click="logout">
              <v-list-item-title>ออกจากระบบ</v-list-item-title>
            </v-list-item>
          </v-list>
        </v-menu>

        <v-list-item-content v-if="!miniVariant">
          <v-list-item-title class="font-weight-bold">{{ userEmail }}</v-list-item-title>
          <v-list-item-subtitle>@username</v-list-item-subtitle> </v-list-item-content>
      </v-list-item>
    </v-navigation-drawer>

    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
      color="white"
      light
      flat
      height="56"
      class="border-bottom"
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" v-if="!permanentDrawer" />
      <v-spacer class="hidden-md-and-up" />

      <v-toolbar-title class="d-flex align-center justify-center flex-grow-1">
        <v-img
          src="/x-logo.png"
          max-height="30"
          max-width="30"
          contain
          class="mr-2"
        ></v-img>
        <span class="font-weight-bold hidden-sm-and-down">หน้าหลัก</span>
      </v-toolbar-title>

      <v-spacer class="hidden-md-and-up" />
      <v-spacer class="hidden-sm-and-down" />

    </v-app-bar>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
    <v-navigation-drawer
      v-model="rightDrawer"
      :right="right"
      temporary
      fixed
      app
    >
      <v-list>
        <v-list-item @click.native="right = !right">
          <v-list-item-action>
            <v-icon light>
              mdi-repeat
            </v-icon>
          </v-list-item-action>
          <v-list-item-title>Switch drawer (click me)</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-footer
      :fixed="fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      clipped: false,
      drawer: true, // ตั้งค่าเริ่มต้นให้ Drawer เปิดอยู่
      fixed: false,
      items: [
        { icon: 'mdi-home', title: 'หน้าหลัก', to: '/' },
        { icon: 'mdi-magnify', title: 'สำรวจ', to: '/explore' }, // เพิ่มสำรวจ
        { icon: 'mdi-bell-outline', title: 'การแจ้งเตือน', to: '/notifications' }, // เพิ่มการแจ้งเตือน
        { icon: 'mdi-newspaper-variant-multiple-outline', title: 'ข่าวสาร', to: '/news' },
        { icon: 'mdi-email-outline', title: 'ข้อความ', to: '/inspire' }, // เพิ่มข้อความ
        { icon: 'mdi-bookmark-outline', title: 'เขียนข้อความ', to: '/mian' }, // เพิ่มที่คั่นหน้า
        { icon: 'mdi-account-group-outline', title: 'ชุมชน', to: '/communities' }, // เพิ่มชุมชน
        { icon: 'mdi-account-outline', title: 'โปรไฟล์', to: '/profile' }, // เพิ่มโปรไฟล์
        { icon: 'mdi-dots-horizontal-circle-outline', title: 'เพิ่มเติม', to: '/more' }, // เพิ่มเพิ่มเติม
        { icon: 'mdi-login', title: 'เข้าสู่ระบบ', to: '/login' } // เก็บ Login ไว้ (อาจจะลบถ้ามีโปรไฟล์แล้ว)
      ],
      miniVariant: true, // ตั้งค่าเริ่มต้นให้ Drawer อยู่ในโหมดไอคอน
      right: true,
      rightDrawer: false,
      title: 'Vuetify.js',
      loggedIn: false,
      userEmail: '',
      userPhotoURL: '',
      permanentDrawer: true // เพิ่ม state สำหรับควบคุม permanent drawer
    }
  },
  mounted() {
    this.$fire.auth.onAuthStateChanged(user => {
      if (user) {
        this.loggedIn = true;
        this.userEmail = user.email;
        this.userPhotoURL = user.photoURL;
      } else {
        this.loggedIn = false;
        this.userEmail = '';
        this.userPhotoURL = '';
      }
    });

    // ตั้งค่า initial miniVariant ตามขนาดหน้าจอ
    this.miniVariant = window.innerWidth < 960; // 960px คือ breakpoint ของ Vuetify สำหรับ md
    // Listener สำหรับ resize
    window.addEventListener('resize', this.handleResize);
  },
  beforeDestroy() {
    // ลบ listener เมื่อ component ถูกทำลาย
    window.removeEventListener('resize', this.handleResize);
  },
  methods: {
    async logout() {
      try {
        await this.$fire.auth.signOut();
        console.log('ออกจากระบบสำเร็จ!');
        this.$router.push('/login');
      } catch (error) {
        console.error('Logout error:', error);
      }
    },
    handleResize() {
      this.miniVariant = window.innerWidth < 960;
    }
  }
}
</script>

<style>
/* CSS สำหรับเส้นแบ่งด้านล่างของ App Bar */
.border-bottom {
  border-bottom: 1px solid #e0e0e0; /* สีเทาอ่อนๆ */
}

/* CSS สำหรับเส้นแบ่งด้านขวาของ Navigation Drawer */
.border-right {
  border-right: 1px solid #e0e0e0; /* สีเทาอ่อนๆ */
}

/* ปรับแต่งปุ่ม V-btn ใน Drawer สำหรับโพสต์ใหม่ */
.v-btn--fab.v-size--x-large {
  height: 60px !important; /* ใหญ่ขึ้นเมื่อขยาย */
  width: 60px !important;
}

.v-btn--fab.v-size--default {
  height: 48px !important; /* เล็กลงเมื่อย่อ */
  width: 48px !important;
}
</style>