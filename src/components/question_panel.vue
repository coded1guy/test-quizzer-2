<template>
    <div class="q_screen">
        {{ qPane }}
    </div>
</template>

<script>
export default {
    name: "questionPanel",
    props: ["qQuestion", "index"],
    data() {
        return {
            qPane: " who is the firstborn son of Angel Michael"
        }
    },
    mounted() {
        this.qPane = this.qQuestion[this.index].question;
        console.log(this.index);
    }
}
</script>

<style scoped>
.q_screen {
    width: 100%;
    padding: 5% 0;
    color: maroon;
    background: white;
    text-align: center;
    font-size: 22px;
    font-family: Verdana, Geneva, Tahoma, sans-serif;
}
</style>