<template>
    <header>
        <h1> {{ title }}</h1>
        <Button 
            v-show="homePage"
            @btn-click="$emit('toggle-add-task')" 
            :text="showAddTask ? 'Close' : 'Add Task'" 
            :color="showAddTask ? 'red' : 'green'" 
        />
    </header>
</template>

<script>
    import Button from "./Button.vue";

    export default {
        name: "Header",
        props: {
            title: {
                type: String,
            },
            showAddTask: {
                type: Boolean
            }
        },
        components: {
            Button
        },
        computed: {
            homePage() {
                let flag = false;
                if (this.$route.path === '/') {
                    flag = true;
                } else {
                    flag = false;
                }
                return flag;
            }
        }
    }
</script>

<style scoped>
    header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>>