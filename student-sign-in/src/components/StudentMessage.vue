<!--
    This is child of App.vue.
    This component needs to be given a student arrived or left information from the parent App.vue.
-->

<template>
    <div>
        <!-- template/html here -->

        <!-- show a message when student arrives or leaves  -->
        <div v-show="student.name">
            <div v-if="student.present" class="alert alert-success">
                Welcome, {{ student.name }}
            </div>

            <div v-else class="alert alert-info">
                Goodbye, {{ student.name }}. See you later!
            </div>
        </div>
    </div>
</template>

<script>
    // export mean another JS file will read this file
    // export default means whatever is created here will be available to the other file
    export default {
        // create component here
        name: 'StudentMessage',
        props: {
            student: Object
        }
    }
</script>

<!-- scoped means the styles will apply only to this component -->
<style scoped>

</style>