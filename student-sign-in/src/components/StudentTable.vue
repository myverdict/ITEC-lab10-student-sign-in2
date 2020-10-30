<!--
    This is the child of App.vue.
    This file is not going to be in charge of getting/creating a list of students.
    This file is going to be given a list of students.
    The job of this file is to display the list of students.
-->

<template>
    <div>
        <!-- template/html here -->
        <div class="card student-list m-2 p-2">
            <h4 class="card-title">Student List</h4>

            <div class="edit-table-toggle form-check">
                <input id="edit-table"
                       type="checkbox"
                       class="form-check-input"
                       v-model="editTable">
                <label for="edit-table" class="form-check-label">Edit table?</label>
            </div>

            <div id="student-table">
                <table class="table">
                    <tr>
                        <th>Name</th>
                        <th>StarID</th>
                        <th>Present?</th>
                        <th v-show="editTable">Delete</th>
                    </tr>

                    <!--
                        Alternate way of writing the below student-row tag:
                        <student-row v-for="student in students" :key="student.starID"></student-row>

                        In the 1st v-bind, the first 'student' is a prop from the child StudentRow.vue, and
                        the value 'student' is the variable from the v-for.

                        v-on is to receive the event from the child component here.
                        In the 1st v-on, the 'student-arrived-or-left' is an event from the child StudentRow.vue, and
                        the value 'arrivedOrLeft' is the method from this component, StudentTable.vue.

                        In the 2nd v-on, the 'delete-student' is an event from the child StudentRow.vue, and
                        the value 'deleteStudent' is the method from this component, StudentTable.vue.
                    -->
                    <student-row v-for="student in students"
                                 v-bind:student="student"
                                 v-bind:key="student.starID"
                                 v-on:student-arrived-or-left="arrivedOrLeft"
                                 v-on:delete-student="deleteStudent"
                                 v-bind:edit="editTable">
                    </student-row>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
    import StudentRow from "@/components/StudentRow.vue";

    // export mean another JS file will read this file
    // export default means whatever is created here will be available to the other file
    export default {
        // create component here
        name: 'StudentTable',
        components: {
            StudentRow
        },
        // this data has to be provided by the parent App.vue
        // not a good practice to modify a prop i.e., remove the v-model from the checkbox input
        props: {
            students: Array
        },
        data() {
            return {
                editTable: false
            }
        },
        methods: {
            arrivedOrLeft(student, present) {
                // TODO emit message to parent App.vue
                this.$emit('student-arrived-or-left', student, present);      // event
            },
            deleteStudent(student) {
                // emits message to the parent App.vue
                this.$emit('delete-student', student);                        // event
            }
        }             // end of methods
    }                 // end of export default
</script>

<!-- scoped means the styles will apply only to this component -->
<style scoped>

</style>