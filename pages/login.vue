<template>
  <v-container fill-height fluid>
    <v-row align="center" justify="center">
      <v-col cols="12" sm="8" md="4">
        <v-card class="elevation-12">
          <v-toolbar color="primary" dark flat>
            <v-toolbar-title>เข้าสู่ระบบ</v-toolbar-title>
            <v-spacer></v-spacer>
          </v-toolbar>
          <v-card-text>
            <v-form ref="form" v-model="valid" lazy-validation>
              <v-text-field
                label="อีเมล"  name="email"
                prepend-icon="mdi-account"
                type="email" v-model="email" :rules="[rules.required, rules.email]"
                required
              ></v-text-field>

              <v-text-field
                label="รหัสผ่าน"
                name="password"
                prepend-icon="mdi-lock"
                type="password"
                v-model="password"
                :rules="[rules.required]"
                required
              ></v-text-field>

              <v-alert
                v-if="error"
                type="error"
                dense
                outlined
                class="mt-3"
              >
                {{ error }}
              </v-alert>
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" @click="submitLogin" :loading="loading">เข้าสู่ระบบ</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'LoginPage',
  layout: 'empty',
  data() {
    return {
      valid: true,
      email: '', // เปลี่ยนเป็น email อย่างเดียว เพื่อให้ตรงกับ Firebase Auth
      password: '',
      loading: false,
      error: null,
      rules: {
        required: value => !!value || 'จำเป็นต้องกรอกข้อมูล',
        email: value => /.+@.+\..+/.test(value) || 'รูปแบบอีเมลไม่ถูกต้อง',
      }
    };
  },
  methods: {
    async submitLogin() {
      if (this.$refs.form.validate()) {
        this.loading = true;
        this.error = null;

        try {
          // ใช้ Firebase Authentication ในการเข้าสู่ระบบ
          await this.$fire.auth.signInWithEmailAndPassword(
            this.email,
            this.password
          );

          console.log('เข้าสู่ระบบสำเร็จด้วย Firebase!');
          this.$router.push('/'); // พาไปหน้า Dashboard หลัง Login สำเร็จ

        } catch (err) {
          console.error('Firebase Login error:', err);
          // แปลง Firebase error code เป็นข้อความที่เข้าใจง่าย
          switch (err.code) {
            case 'auth/user-not-found':
              this.error = 'ไม่พบผู้ใช้นี้';
              break;
            case 'auth/wrong-password':
              this.error = 'รหัสผ่านไม่ถูกต้อง';
              break;
            case 'auth/invalid-email':
              this.error = 'รูปแบบอีเมลไม่ถูกต้อง';
              break;
            default:
              this.error = 'เกิดข้อผิดพลาดในการเข้าสู่ระบบ: ' + err.message;
          }
        } finally {
          this.loading = false;
        }
      }
    },
    // ถ้าคุณมีหน้าสมัครสมาชิก คุณจะใช้ await this.$fire.auth.createUserWithEmailAndPassword(email, password)
  }
};
</script>