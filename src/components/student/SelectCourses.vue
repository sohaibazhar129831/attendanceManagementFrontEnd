<template>
  <div class="mt-10">
    <v-select
      :items="Courses"
      label="Choose Course"
      outlined
      v-model="id"
      item-text="name"
      item-value="_id"
      @change="fetchStudentAttendance()"
    ></v-select>
    <v-divider></v-divider>
    <v-data-table dark :items="Students" :headers="headers"></v-data-table>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "SelectCourses",
  data() {
    return {
      selected: "none",
      headers: [
        { text: "Name", value: "student.name" },
        { text: "Date", value: "date" },
        { text: "Status", value: "isPresent" },
      ],
      Courses: [],
      Students: [],
      id: "",
    };
  },
  async mounted() {
    const loggedInUserId = this.$cookie.get("loggedInUserId");
    var response = await axios.get(`http://127.0.0.1:5000/api/courses/students/${loggedInUserId}`);
    this.Courses = response.data;
  },
  methods: {
    async fetchStudentAttendance() {
      const loggedInUserId = this.$cookie.get("loggedInUserId");
      const response2 = await axios.get(`http://127.0.0.1:5000/api/attendance/studentcourse/${this.id}&${loggedInUserId}`);
      console.log(response2.data)
      this.Students = response2.data;
    },
  },
};
</script>