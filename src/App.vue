<template>
    <div id="app">
        <DueDate title="Due Date Calculator" />
    </div>
</template>

<script>
    import DueDate from "./components/DueDate.vue";

    export default {
        name: "App",
        components: {
            DueDate,
        },
        setup() {

        }
    };
</script>

<style>
</style>
