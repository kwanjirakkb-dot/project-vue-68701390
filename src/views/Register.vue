<template>
  <div class="register-container">
    <h3 class="register-title">Register</h3>

    <form @submit.prevent="handleSubmit">
      <div class="mb-3">
        <label class="form-label">ชื่อ</label>
        <input type="text" class="form-control" v-model="form.firstname" />
      </div>

      <div class="mb-3">
        <label class="form-label">นามสกุล</label>
        <input type="text" class="form-control" v-model="form.lastname" />
      </div>

      <div class="mb-3">
        <label class="form-label">เบอร์โทรศัพท์</label>
        <input type="tel" class="form-control" v-model="form.phone" />
      </div>

      <div class="mb-3">
        <label class="form-label">Username</label>
        <input type="text" class="form-control" v-model="form.username" />
      </div>

      <div class="mb-3">
        <label class="form-label">Password</label>
        <input type="text" class="form-control" v-model="form.password" />
      </div>

      <button type="submit" class="btn btn-primary w-100">
        บันทึก
      </button>
    </form>

    <!-- Debug -->
    <div class="mt-4" v-if="submitted">
      <h5>ข้อมูลที่ส่ง:</h5>
      <pre>{{ form }}</pre>
    </div>
  </div>
</template>

<script>
export default {
  name: "RegisterView",
  data() {
    return {
      form: {
        firstnamename: "",
        lastname: "",
        phone: "",
        username: "",
        password: "",
      },
      submitted: false,
    };
  },
  methods: {
    handleSubmit() {
      fetch("http://localhost/api/insert_customer.php", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(this.form)
      })
      .then(res => res.json())
      .then(data => {
        console.log(data);
        alert("สมัครสมาชิกสำเร็จ");
      });
    }
  }
};
</script>

<style scoped>
.register-container {
  max-width: 500px;
  margin: 80px auto;
}

.register-title {
  text-align: center;
  margin-bottom: 30px;
}
</style>
