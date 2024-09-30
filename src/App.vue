<template>
  <div id="app" class="container">
    <input
      style="width: 300px; height: 30px"
      v-model="newJob"
      placeholder="Enter your job"
    />
    <button
      style="background-color: blue; width: 80px; height: 38px"
      @click="addJob"
    >
      Add Job
    </button>

    <div v-for="(job, index) in jobs" :key="index" class="job-item">
      <input type="checkbox" v-model="job.completed" @change="updateJobs" />
      <span :class="{ completed: job.completed }">{{ job.text }}</span>
      <button @click="deleteJob(index)">Delete</button>
    </div>

    <div class="status">
      <p>
        Số công việc hoàn thành: {{ completedJobsCount }} /
        {{ jobs.length }} công việc
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";

// Lấy dữ liệu từ localStorage hoặc khởi tạo dữ liệu rỗng
const savedJobs = JSON.parse(localStorage.getItem("jobs")) || [];

// Biến reactive lưu trữ các công việc
const newJob = ref("");
const jobs = ref(savedJobs);

// Đếm số công việc đã hoàn thành
const completedJobsCount = computed(() => {
  return jobs.value.filter((job) => job.completed).length;
});

// Thêm công việc mới
const addJob = () => {
  if (newJob.value) {
    jobs.value.push({ text: newJob.value, completed: false });
    newJob.value = "";
    updateJobs(); // Cập nhật vào localStorage
  }
};

// Xóa công việc
const deleteJob = (index) => {
  jobs.value.splice(index, 1);
  updateJobs(); // Cập nhật vào localStorage
};

// Cập nhật localStorage khi có thay đổi
const updateJobs = () => {
  localStorage.setItem("jobs", JSON.stringify(jobs.value));
};

// Khi ứng dụng khởi chạy, đảm bảo dữ liệu được load
onMounted(() => {
  const storedJobs = JSON.parse(localStorage.getItem("jobs"));
  if (storedJobs) {
    jobs.value = storedJobs;
  }
});
</script>

<style scoped>
.container {
  max-width: 400px;
  margin: auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.job-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 10px 0;
}

.completed {
  text-decoration: line-through;
}

button {
  background-color: red;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
}

button:hover {
  opacity: 0.8;
}
</style>
