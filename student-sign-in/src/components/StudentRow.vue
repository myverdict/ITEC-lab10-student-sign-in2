<!--
    This component is the child of StudentTable.vue.
-->

<template>
    <!--
        Create table rows for table data.
        Each row will have a checkbox, bound to the app's data.
        When the checkbox is checked/unchecked, the student will be signed in/out.

        Alternate way of writing the below opening tr tag:
        <tr :key="student.starID" :class="student.present ? 'present' : 'absent'">
        <tr v-bind:key="student.starID" v-bind:class="student.present ? 'present' : 'absent'">
    -->
    <tr v-bind:class="{ present: student.present, absent: !student.present }">
        <td>{{ student.name }}</td>
        <td>{{ student.starID }}</td>
        <!--
            Delete the v-model so that the parent App.vue controls the child component
            <td><input type="checkbox" v-model="student.present" @change="arrivedOrLeft(student)"></td>
            <td><input type="checkbox" v-model="student.present" v-on:change="arrivedOrLeft(student)"></td>

            Below in $event.srcElement.checked, the srcElement is deprecated
            <td><input type="checkbox"
                       v-bind:checked="student.present"
                       v-on:change="arrivedOrLeft(student, $event.srcElement.checked)"></td>
            Use $event.target.checked instead
        -->
        <td><input type="checkbox"
                   v-bind:checked="student.present"
                   v-on:change="arrivedOrLeft(student, $event.target.checked)"></td>
        <!-- the edit is a prop in this component -->
        <td v-show="edit"><img src="@/assets/delete.png"
                 v-on:click="deleteStudent"></td>
    </tr>
</template>

<script>
    export default {
        name: 'StudentRow',
        props: {
            student: Object,
            edit: Boolean
        },
        methods: {
            arrivedOrLeft(student, present) {
                // emit message to parent StudentTable.vue
                this.$emit('student-arrived-or-left', student, present);      // event
            },
            deleteStudent() {
                // this component does not have the power to delete the student.
                // it should tell its' parent StudentTable.vue which tells its' parent App.vue to delete the student.
                if(confirm(`Delete ${this.student.name}?`))
                {
                    this.$emit('delete-student', this.student);                 // event
                }
            }
        }
    }
</script>

<!-- scoped means the styles will apply only to this component -->
<style scoped>
    .present { color: lightgrey; font-style: italic; }
    .absent { color: black; font-weight: bold }

    /* set delete.png icon size */
    img { height: 25px; }
</style>