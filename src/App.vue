<template>
  <div class="container">
    <div class="d-flex justify-content-center align-items-center mt-5">
      <b-button pill variant="outline-danger" class="mx-3" @click="viewCourse = true">Courses</b-button>
      <b-button pill variant="outline-danger" @click="viewCourse = false">Students</b-button>
      <strong class="text-danger ms-3" v-if="items.length == 0 && !viewCourse">You currently do not have any students !!!</strong>
      <b-button pill variant="outline-success" v-b-modal.modal-1 class="mx-3" @click="çreateStudent()"
        v-if="!viewCourse || items.length == 0">Add New Student</b-button>
    </div>
    <CourseList :viewCourse="viewCourse" />
    <DetailStudent :viewCourse="viewCourse" :items="items" @editStudent="this.editStudent" @changeID="this.changeId" />
    <CreateAndEditStudent :items="items" :title="title" :viewCourse="viewCourse" :form="form" :id="id" />
    <DeleteStudent :items="items" :id="id" />
  </div>
</template>
<script>
import axios from "axios"
import DetailStudent  from "./components/DetailStudent.vue"
import CourseList from "./components/CourseList.vue"
import DeleteStudent from "./components/DeleteStudent.vue"
import CreateAndEditStudent from "./components/CreateAndEditStudent.vue"
export default {
  components: {
    CreateAndEditStudent,
    CourseList,
    DeleteStudent,
    DetailStudent
  },
  data() {
    return {
      items: [],
      viewCourse: false,
      id: null,
      title: '',
      form: {
        index_number: '',
        first_name: '',
        last_name: '',
        year: '',
        status: null
      }
    }
  },
  methods:{
    changeId(id){
      this.id = id
    },
    çreateStudent() {
      this.title = 'Add New Student'
      this.form.index_number = ''
      this.form.first_name = ''
      this.form.last_name = ''
      this.form.year = ''
      this.form.status = null
    },
    editStudent(item) {
      this.title = "Edit Student"
      this.id = item.id
      this.form.index_number = item.index_number
      this.form.first_name = item.first_name
      this.form.last_name = item.last_name
      this.form.year = item.year
      this.form.status = item.status == 'Part Time' ? 'pt' : 'ft'
    }
  },
  async created() {
    axios.get("api/students/").then((data) => {
      for (let i in data.data) {
        axios.get(`api/students/${data.data[i].id}/courses/`).then((res) => {
          data.data[i].courses = res.data;
          this.items.push(data.data[i])
          this.items[i].status == 'pt' ? 
          this.items[i].status = 'Part Time' :
          this.items[i].status = 'Full Time'
        })
      }
    })
  }
}
</script>




