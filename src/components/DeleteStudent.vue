<template>
        <b-modal id="modal-2" title="Confirm Delete" ok-title="Remove" @ok="confirmDeleteStudent()">
            <p class="my-4">Are you sure you want to remove a student ?</p>
        </b-modal>
</template>
<script>
import axios from 'axios'
export default {
    props: ['items', 'id'],
    methods: {
        confirmDeleteStudent() {
            axios.delete(`api/students/${this.id}`, this.id).then((data) => {
                if (data.status == 204) {
                    for (let i in this.items) {
                        if (this.items[i].id == this.id) {
                            if (i > -1) {
                                this.items.splice(i, 1);
                            }
                        }
                    }
                }
            })
        }
    }
}
</script>
