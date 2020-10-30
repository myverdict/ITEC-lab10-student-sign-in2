<!-- this is the parent of NewStudentForm.vue, StudentTable.vue, and NewStudentForm.vue -->

<template>
    <div id="app">
        <!-- add the 3 components -->
        <!--
            In v-on, 'student-added' is an event from the 'addStudent' method of the child NewStudentForm.vue,
            and the value 'newStudentAdded' method is from the parent App.vue.
        -->
        <new-student-form v-on:student-added="newStudentAdded"></new-student-form>

        <!--
            Send the students information to the child StudentTable.vue by using v-bind.

            In v-bind, the first 'students' is the name of the prop from the child StudentTable.vue,
            and the value/second 'students' is data from the parent App.vue.

            In the 1st v-on, the 'student-arrived-or-left' is an event from the 'arrivedOrLeft' method of the
            child StudentTable.vue,
            and the value 'studentArrivedOrLeft' method is from the parent App.vue.

            In the 2nd v-on, the 'delete-student' is an event from the 'deleteStudent' method of the
            child StudentTable.vue, which is getting this event from its' child the StudentRow.vue component.
            and the value 'studentDeleted' method is from the parent App.vue.
        -->
        <student-table v-bind:students="students"
                       v-on:student-arrived-or-left="studentArrivedOrLeft"
                       v-on:delete-student="studentDeleted"></student-table>

        <!--
            Send the updated student information to the child StudentMessage.vue by using v-bind.

            In v-bind, the first 'student' is the name of the prop from the child StudentMessage.vue,
            and the value 'mostRecentStudent' is data from the parent App.vue.
        -->
        <student-message v-bind:student="mostRecentStudent"></student-message>
    </div>
</template>

<script>
    import NewStudentForm from './components/NewStudentForm.vue'
    import StudentMessage from './components/StudentMessage.vue'
    import StudentTable from './components/StudentTable.vue'

    export default {
        name: 'App',
        components: {
            NewStudentForm,
            StudentMessage,
            StudentTable
        },
        data() {
            return {
                students: [],                   // initial empty array
                mostRecentStudent: {}           // initial empty object
            }
        },
        methods: {
            newStudentAdded(student) {
                this.students.push(student);
                this.students.sort(function(s1, s2) {
                    // returns 1 if s1 should be after s2
                    // returns -1 if s1 should be before s2
                    return s1.name.toLowerCase() < s2.name.toLowerCase() ? -1 : 1;
                })
            },
            studentArrivedOrLeft(student, present) {
                /*
                    TODO find student in this.students, set present value.
                    the find function will return the 1st match if there is a match,
                    if there no match, then find will return undefined.
                */
                let updateStudent = this.students.find( function(s) {
                    if(s.name === student.name && s.starID === student.starID)
                    {
                        return true;
                    }
                })

                if(updateStudent)
                {
                    updateStudent.present = present;
                    this.mostRecentStudent = student;
                }
            },
            studentDeleted(student) {
                // filter returns a new array of all students for whom the function returns true
                this.students = this.students.filter( function(s) {
                    if(s != student)
                    {
                        return true;
                    }
                })

                // TODO clear welcome/goodbye
                this.mostRecentStudent = {};
            }
        }
    }
</script>

<style>

</style>
