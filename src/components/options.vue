<template>
    <div class="op_screen">
        <div ref="option1" class="optn" id="optn1" @click="$emit('chosen-option', optOne)" v-on:click="progress">
            <span>A.</span>
            <div>
                <span id="op1_check">{{ optOne }}</span>
            </div>
        </div>
        <div ref="option2" class="optn" id="optn2" @click="$emit('chosen-option', optTwo)">
            <span>B.</span>
            <div>
                <span id="op2_check">{{ optTwo }}</span>
            </div>
        </div>
        <div ref="option3" class="optn" id="optn3" @click="$emit('chosen-option', optThree)">
            <span>C.</span>
            <div>
                <span id="op3_check">{{ optThree }}</span>
            </div>
        </div>
    </div>
</template>


<script>
export default {
    name: "options",
    props: ["qOptions", "index"],
    data() {
        return {
            //options: document.querySelectorAll('.opt'),
            optOne: "David",
            optTwo: "Saul",
            optThree: "Samuel",
            optArr: [ this.$refs.option1, this.$refs.option2, this.$refs.option3 ]
        }
    },
    methods: {
        // FUNCTION TO PASS IN THE DATA FROM THE BIG ARRAY TO THEIR RESPECTIVE DATA FIELD
        assignOptionsValues() {
            this.optOne = this.qOptions[this.index].option1;
            this.optTwo = this.qOptions[this.index].option2;
            this.optThree = this.qOptions[this.index].option3;
            //this.ans = questions[this.index].answer;
        },
        assignOptionColors() {
            // this.$refs.option1.style.background = "rgb(163, 163, 68)";
            // this.$refs.option2.style.background = "rgb(163, 163, 68)";
            // this.$refs.option3.qOption3.style.background = "red";

            this.optArr[0].style.background = "rgb(163, 163, 68)";
            this.optArr[1].style.background = "rgb(163, 163, 68)";
            this.optArr[2].style.background = "red";
        },
        chosen(name) {
            console.log(name);
        },
        progress() {}
    },
    created() {
        this.assignOptionsValues()
    },
    mounted() {
        console.log(this.$refs)
    }
}
</script>


<style scoped>
.op_screen {
    width: 100%;
    display: flex;
    flex-direction: column;
    padding: 3% 0;
    font-size: 17px;
    font-family: Verdana, Geneva, Tahoma, sans-serif;
}
.optn span {
    margin-right: 10px;
}
.optn {
    width: 90%;
    margin: 0 5% 2% 5%;
    display: flex;
    flex-direction: row;
    background: rgb(163, 163, 68);
    color: #fff;
    border-radius: 5px;
    padding: 8px 26px;
    font-size: 17px;
    cursor: pointer;
    transition: .5s ease-in-out;
}
.optn:hover {
    background: rgb(116, 116, 43);
}
</style>