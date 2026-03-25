<template>
  <div class="container my-5">
    <h2>แสดงข้อมูลพนักงาน</h2>
    <table class="table table-hover">
      <thead class="table table-primary table-hover">
        <tr>
          <th>รหัสพนักงาน</th>   
          <th>ชื่อ-นามสกุล</th>           
          <th>แผนก</th>        
          <th>เงินเดือน</th>       
          <th>สถานะ</th>      
          <th>วันที่ทำรายการ</th>
        </tr>
      </thead>

      <tbody>
        <!-- วนลูปข้อมูล customers -->
        <tr v-for="(item,) in employees" :key="item.emp_id">
         
          <td>{{ item.emp_id }}</td> 
          <td>{{ item.full_name }}</td>  
          <td>{{ item.department }}</td>  
          <td>{{ item.salary }}</td>           
          <td>
          <span v-if="item.active==1">ปกติ</span>
          <span v-else>ลาออก</span>
        </td>
        <td>{{ item.created_at }}</td>
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
  name: "EmployeesList", // ชื่อ component

  setup() {
    // -----------------------------
    // state (ตัวแปร reactive)
    // -----------------------------
    const employees = ref([]); // เก็บข้อมูลลูกค้า (array)
    const loading = ref(true); // สถานะโหลดข้อมูล
    const error = ref(null);   // เก็บ error

    // -----------------------------
    // ฟังก์ชันดึงข้อมูลจาก API
    // -----------------------------
    const fetchdata = async () => {
      try {
        // เรียก API (PHP)
        const response = await fetch("http://localhost/project-vue-68701390/php_api/show_employees.php");

        // ตรวจสอบว่าการเรียกสำเร็จหรือไม่
        if (!response.ok) {
          throw new Error("ไม่สามารถดึงข้อมูลได้");
        }

        // แปลง response เป็น JSON
        employees.value = await response.json();

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
      employees,
      loading,
      error
    };
  }
};
</script>