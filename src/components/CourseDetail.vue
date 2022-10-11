<template>
    <div class="d-flex justify-content-center align-items-center">
        <div v-if="viewCourse">
            <b-badge variant="primary" class="mx-1" @click="studentview = !studentview">{{course.name}}
                <div v-if="studentview">
                    <b-badge variant="success" class="mx-1" v-for="student in course.students" :key="student.id">
                        {{student.first_name}}- {{student.last_name}}
                    </b-badge>
                </div>
            </b-badge>
        </div>
    </div>
</template>
<script>
export default {
    props: ['viewCourse', 'course'],
    data() {
        return {
            studentview: false
        }
    }
}
</script>
