<template>
  <div>
    <b-modal id="modal-1" :title="title" @ok="createOrEditStudent">
      <ValidationObserver ref="student-observer">
        <validation-provider rules="required" v-slot="{ errors }">
          <label class="mt-4">Index Number</label>
          <input type="text" class="form-control" v-model="form.index_number">
          <p class="error-msg text-danger">{{ errors[0] }}</p>
        </validation-provider>
        <label class="mt-4">First Name</label>
        <input type="text" class="form-control" v-model="form.first_name">
        <label class="mt-4">Last Name</label>
        <input type="text" class="form-control" v-model="form.last_name">
        <label class="mt-4">Year</label>
        <input type="text" class="form-control" v-model="form.year">
        <validation-provider rules="required" v-slot="{ errors }">
          <label class="mt-4">Select status</label>
          <b-form-select v-model="form.status" :options="options" class="form-select"></b-form-select>
          <p class="error-msg text-danger">{{ errors[0] }}</p>
        </validation-provider>
      </ValidationObserver>
    </b-modal>
  </div>
</template>
<script>
import axios from "axios"
import { ValidationProvider, ValidationObserver, extend } from "vee-validate";
import { required } from "vee-validate/dist/rules";
extend("required", {
  ...required,
  message: "This field is required !",
});
export default {
  props: ['items', 'title', 'viewCourse', 'form', 'id'],
  components: {
    ValidationProvider,
    ValidationObserver
  },
  data() {
    return {
      students: this.items,
      options: [
        { value: 'pt', text: 'Part Time' },
        { value: 'ft', text: 'Full Time' },
      ],
    }
  },
  methods: {
    async createOrEditStudent(event) {
      event.preventDefault()
      const isvalid = await this.$refs['student-observer'].validate();
      if (!isvalid) {
        return;
      }
      if (this.title == 'Add New Student') {
        axios.post("api/students/", this.form).then((data) => {
          this.students.push(data.data)
          for (let i in this.students) {
            this.students[i].status == 'pt' ?
              this.students[i].status = 'Part Time' :
              this.students[i].status = 'Full Time'
          }
          if (this.students.length > 0) {
            this.viewCourse = false
          }
        })
        this.$bvModal.hide("modal-1")
      }
      if (this.title == 'Edit Student') {
        axios.put(`api/students/${this.id}/`, this.form).then((data) => {
          const index = this.students.findIndex(student => student.id === data.data.id);
          this.students.splice(index, 1, data.data);
          for (let i in this.students) {
            this.students[i].status == 'pt' ?
              this.students[i].status = 'Part Time' :
              this.students[i].status = 'Full Time'
          }
        })
        this.$bvModal.hide("modal-1")
      }
    },
  }
}
</script>
