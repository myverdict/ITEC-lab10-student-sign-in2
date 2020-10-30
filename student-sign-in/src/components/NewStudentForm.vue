<!--
    This is child of App.vue.
    The job of this component is to be a form, display a form, validate the form, and tell
    something about the new student.
-->

<template>
    <div>
        <!-- template/html here -->
        <!-- show errors from form validation -->
        <div class="alert alert-danger" v-show="errors.length > 0">
            <ul>
                <li v-for="error in errors" v-bind:key="error">
                    {{ error }}
                </li>
            </ul>
        </div>

        <!-- form section -->
        <div class="card add-student m-2 p-2">
            <h4 class="card-title">Add new student</h4>

            <div class="form-group">
                <label for="name">Name</label>

                <!-- v-model newStudentName -->
                <input id="name" class="form-control" v-model.trim="newStudentName">
            </div>

            <div class="form-group">
                <label for="starID">Star ID</label>

                <!-- v-model newStarID -->
                <input id="starID" class="form-control" v-model.trim="newStarID">
            </div>

            <!-- v-on:click event handler -->
            <!-- <button class="btn btn-primary" @click="addStudent">Add</button> -->
            <button class="btn btn-primary" v-on:click="addStudent">Add</button>
        </div>
    </div>
</template>

<script>
    // export mean another JS file will read this file
    // export default means whatever is created here will be available to the other file
    export default {
        // create component here
        name: 'NewStudentForm',
        data() {
            return {
                newStudentName: '',       // initial value
                newStarID: '',            // initial value
                errors: []                // initial empty array
            }
        },
        methods: {
            addStudent() {
                this.errors = [];

                if(!this.newStudentName)
                {
                    this.errors.push("Student name is required.");
                }

                if(!this.newStarID)
                {
                    this.errors.push("Star ID is required.");
                }

                if(this.errors.length === 0)
                {
                    let student = { name: this.newStudentName, starID: this.newStarID, present: false };

                    // TODO emit message to parent App.vue with new student
                    this.$emit('student-added', student);               // event
                    this.newStudentName = "";
                    this.newStarID = "";
                }
            },            // end of addStudent method inside methods
        }                 // end of methods
    }                     // end of export default
</script>

<!-- scoped means the styles will apply only to this component -->
<style scoped>

</style>