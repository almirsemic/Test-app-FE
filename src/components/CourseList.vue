<template>
    <div class="mt-5">
        <CourseDetail :viewCourse="viewCourse" v-for="course in courses" :key="course.id" :course="course" />
    </div>
</template>
<script>
import axios from "axios"
import CourseDetail from "../components/CourseDetail.vue"
export default {
    props: ['viewCourse'],
    components: {CourseDetail},
    data() {
        return {
            courses: []
        }
    },
    async created() {
        const courses = await axios.get('api/courses/');
        for (let course of courses.data) {
            const students = await axios.get(`api/courses/${course.id}/students/`);
            course.students = students.data;
        }
        this.courses = courses.data;
    }
}
</script>
