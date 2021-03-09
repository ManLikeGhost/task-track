<template>
    <header>
       <h1>{{title}}</h1> 
       <Button 
        @btn-click="$emit('toggle-add-task')" 
        :text="displayAddTask ? 'Close Me' : 'Add new'" 
        :color="displayAddTask ? 'red' : 'purple'" 
        />
    </header>
</template>

<script>

    import Button from './Button'


    export default {
        name: 'Header',
        props: {
            title: String,
            displayAddTask: Boolean,
        },
        components: {
            Button
        }
    }
</script>

<style scoped>
    header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom:20px;
    }
</style>