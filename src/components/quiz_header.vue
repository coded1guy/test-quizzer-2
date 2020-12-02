<template>
    <header v-on:refresh-index="startCounting">
        <span class="category">
            category: {{ category }}
        </span>
        <span class="atmpt_qstn">
            Attempted Questions: {{ ids }}
        </span>
        <span class="timer">
            Timer: {{ counter }}
        </span>
    </header>
</template>

<script>
export default {
    name: "quizHeader",
    props: ["qHeader", "index"],
    data() {
        return {
            currentCount: 10,
            timer: "",
            checkId: 0,
            result: "",
            id: 1, 
            resVal: 0,

            // DOM ELEMENTS
            category: "",
            ids: "",
            counter: 0
        }
    }, 
    methods: {
        // FUNCTION TO PASS IN THE DATA FROM THE BIG ARRAY TO THEIR RESPECTIVE DATA FIELD
        assignQuizHeaderValues() {
            this.category = this.qHeader[this.index].category.toUpperCase();
            this.ids = `${this.index + 1} / ${this.qHeader.length}`;
        },
        startCounting() {
            this.counter = 10;
            this.currentCount = 10;
            this.timer = setInterval(() => {
                this.currentCount--;
                this.counter = this.currentCount;
                if(this.currentCount === 0) {
                    this.$emit('clicked-button', 'skip');
                    clearInterval(this.timer);
                    setTimeout(() => {
                        this.counter = 10;
                        this.currentCount = 10;
                    }, 1000);
                }
            }, 1000);
        },
        stopCountingPatapata() {
            clearInterval(this.timer);
            this.counter = 10;
        }
    },
    mounted() {
        this.assignQuizHeaderValues(),
        this.startCounting()
    }
    // updated() {
    //     this.assignQuizHeaderValues(),
    //     this.startCounting()
    // }
}
</script>

<style scoped>
header {
    width: 100%;
    font-size: 15px;
    color: rgb(3, 97, 97);
    padding: 2% 3%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-family: Verdana, Geneva, Tahoma, sans-serif;
}
</style>
