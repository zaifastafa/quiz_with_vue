<template>
    <div class="header-container">
        <b-nav tabs>
            <b-nav-item disabled><b>Quiz App</b></b-nav-item>
            <b-nav-item disabled>Question: {{ numCorrect }}/ {{ numTotal}}</b-nav-item>
        </b-nav>
    </div>
</template>

<script>
    export default {
        name: "Header",
        props: [
            'numCorrect',
            'numTotal'
        ]
    }
</script>

<style scoped>

</style>
