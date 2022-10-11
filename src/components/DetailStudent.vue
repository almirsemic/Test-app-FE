<template>
    <div class="mt-5" v-if="items.length > 0 && !viewCourse">
        <b-table hover :items="items" :fields="fields">
            <template v-slot:cell(courses)="{ item }">
                <div v-for="course in item.courses">
                    {{course.name}}
                </div>
            </template>
            <template v-slot:cell(actions)="{ item }">
                <div>
                    <b-button v-b-modal.modal-1 variant="success" @click="$emit('editStudent', item)">Edit</b-button>
                    <b-button class="mx-1" variant="danger" v-b-modal.modal-2 @click="$emit('changeID', item.id)">Remove</b-button>
                </div>
            </template>
        </b-table>
    </div>
</template>
<script>
export default {
    props: ['viewCourse', 'items'],
    data() {
        return {
            fields: ['index_number', 'first_name', 'last_name', 'year', 'status', 'courses', 'actions'],
        }
    }
}
</script>
