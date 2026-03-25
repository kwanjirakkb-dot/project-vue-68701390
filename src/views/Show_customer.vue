<template>
  <div class="container my-5">
    <h2>แสดงข้อมูลลูกค้า</h2>
    <table class="table table-hover">
      <thead class="table table-primary table-hover">
        <tr>
          <th>รหัสลูกค้า</th>   
          <th>ชื่อ</th>           
          <th>นามสกุล</th>        
          <th>เบอร์โทร</th>       
          <th>Username</th>      
        </tr>
      </thead>

      <tbody>
        <!-- วนลูปข้อมูล customers -->
        <tr v-for="(item,index) in customers" :key="item.customer_id">
         
          <td>{{ item.customer_id }}</td> 
          <td>{{ item.firstName }}</td>  
          <td>{{ item.lastName }}</td>    
          <td>{{ item.phone }}</td>       
          <td>{{ item.username }}</td>  
        </tr>
      </tbody>
    </table>

    <!-- Loading: แสดงระหว่างรอข้อมูล -->
    <div v-if="loading" class="text-center">
      <p>กำลังโหลดข้อมูล...</p>
    </div>

    <!-- Error: แสดงเมื่อเกิดข้อผิดพลาด -->
    <div v-if="error" class="alert alert-danger">
      {{ error }}
    </div>
  </div>
</template>

<script>
// import ฟังก์ชันจาก Vue (Composition API)
import { ref, onMounted } from "vue";

export default {
  name: "CustomerList", // ชื่อ component

  setup() {
    // -----------------------------
    // state (ตัวแปร reactive)
    // -----------------------------
    const customers = ref([]); // เก็บข้อมูลลูกค้า (array)
    const loading = ref(true); // สถานะโหลดข้อมูล
    const error = ref(null);   // เก็บ error

    // -----------------------------
    // ฟังก์ชันดึงข้อมูลจาก API
    // -----------------------------
    const fetchdata = async () => {
      try {
        // เรียก API (PHP)
        const response = await fetch("http://localhost/project-vue-68701390/php_api/show_customer.php");

        // ตรวจสอบว่าการเรียกสำเร็จหรือไม่
        if (!response.ok) {
          throw new Error("ไม่สามารถดึงข้อมูลได้");
        }

        // แปลง response เป็น JSON
        customers.value = await response.json();

      } catch (err) {
        // ถ้า error ให้เก็บข้อความไว้แสดง
        error.value = err.message;

      } finally {
        // ไม่ว่าจะสำเร็จหรือ error ให้หยุด loading
        loading.value = false;
      }
    };

    // -----------------------------
    // lifecycle: ทำงานเมื่อ component โหลดเสร็จ
    // -----------------------------
    onMounted(() => {
      fetchdata(); // เรียก API ทันที
    });

    // -----------------------------
    // return ค่าไปใช้ใน template
    // -----------------------------
    return {
      customers,
      loading,
      error
    };
  }
};
</script>