<template>
    <main id="main">
        <!--HEADER HTML-->
        <header>
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
        <div class="quiz_cont">
            <!--PROGRESS HTML-->
            <div class="progress">
                <span class="moving_color"></span>
            </div>
            <!--QUESTIONS HTML-->
            <div class="q_screen">
                {{ qPane }}
            </div>
            <!--OPTIONS HTML-->
            <div class="op_screen">
                <div class="optn" :style="{ background: op1Bg }" @click="validate(optOne)" v-on:click="progress">
                    <span>A.</span>
                    <div>
                        <span id="op1_check">{{ optOne }}</span>
                    </div>
                </div>
                <div class="optn" :style="{ background: op2Bg }" @click="validate(optTwo)">
                    <span>B.</span>
                    <div>
                        <span id="op2_check">{{ optTwo }}</span>
                    </div>
                </div>
                <div class="optn" :style="{ background: op2Bg }" @click="validate(optThree)">
                    <span>C.</span>
                    <div>
                        <span id="op3_check">{{ optThree }}</span>
                    </div>
                </div>
            </div>
            <!--BUTTONS HTML-->
            <div class="btn_cnt" :style="{ justifyContent: btnCntJus }" >
                <button :style ="{ display: prevDis }" style="display: none;" @click="getButton('previous')">previous</button>
                <button :style ="{ display: nextDis }" @click="getButton('next')">next</button>
                <button :style ="{ display: skipDis }" @click="getButton('skip')">skip</button>
                <button :style ="{ display: finishDis }" @click="getButton('finish')">finish</button>
                <button :style ="{ display: refreshDis }" @click="getButton('refresh')">start again</button>
            </div>
        </div>
    </main>
</template>

<script>
export default {
    name: "MainCnt",
    props: ["bigData"],
    data() {
        return {
            mainEl: document.querySelector('#main'),
            //index: 0,
            answer: [], 
            cAns: [], 
            checkId: 0,
            result: "", 
            ans: "", 
            optionChosen: "",
            i: 0, 
            id: 1, 
            resVal: 0,
            // DATA FOR THE QUIZ HEADER
            currentCount: 10,
            timer: "",
            category: "none",
            ids: "0",
            counter: 0,
            // DATA FOR QUESTIONS
            qPane: " who am I?",
            // DATA FOR THE OPTIONS
            optOne: "omo",
            optTwo: "emmo",
            optThree: "hmm",
            optArr: [ this.$refs.option1, this.$refs.option2, this.$refs.option3 ],
            // STYLES DATA
            prevDis: "none",
            nextDis: "block",
            skipDis: "block",
            finishDis: "none",
            refreshDis: "none",
            btnCntJus: "space-between", 
            op1Bg: "rgb(163, 163, 68)",
            op2Bg: "rgb(163, 163, 68)",
            op3Bg: "rgb(163, 163, 68)"
        }
    },
    methods: {  
        // FUNCTION TO PASS IN THE DATA FROM THE BIG ARRAY TO THEIR RESPECTIVE DATA FIELD
        assignValues() {
            this.category = this.bigData[this.i].category.toUpperCase();
            this.ids = `${this.i + 1} / ${this.bigData.length}`;

            this.qPane = this.bigData[this.i].question;
            // OPTION VALUES
            this.optOne = this.bigData[this.i].option1;
            this.optTwo = this.bigData[this.i].option2;
            this.optThree = this.bigData[this.i].option3;

            this.assignOptionColors();
            this.updateButton();
        },
        startCounting() {
            this.counter = 10;
            this.currentCount = 10;
            this.timer = setInterval(() => {
                this.currentCount--;
                this.counter = this.currentCount;
                if(this.i < this.bigData.length && this.currentCount === 0) {
                    this.getButton('skip');
                    console.log('skipped');
                } else if(this.i === this.bigData.length && this.currentCount === 0) {
                    this.getButton('finish');
                    console.log('finished');
                }
            }, 1000);
        },
        stopCounting() {
            clearInterval(this.timer);
            this.counter = 10;
            this.currentCount = 10;
        },
        // OPTIONS SCRIPT
        validate(option) {
            console.log(option);
        },
        assignOptionColors() {
            this.op1Bg = "rgb(163, 163, 68)";
            this.opt2Bg = "rgb(163, 163, 68)";
            this.op3Bg = "rgb(163, 163, 68)";
        },
        chosen(name) {
            console.log(name);
        },
        progress() {},

        // BUTTONS SCRIPT
        updateButton() {
            console.log('start update button method');
            if(this.i === 0) {
                console.log('i = 0');
                this.prevDis = "none";
                this.nextDis = "block";
                this.skipDis = "block";
                this.finishDis = "none";
                this.refreshDis = "none";
            }
            if (this.i < this.bigData.length && this.i > 0) {
                console.log('i is less than index and greater than 0')
                this.prevDis = "block";
                this.nextDis = "block";
                this.skipDis = "block";
                this.finishDis = "none";
                this.refreshDis = "none";
            }
            if (this.i === this.bigData.length) {
                console.log('i = index');
                this.prevDis = "block";
                this.nextDis = "none";
                this.skipDis = "none";
                this.finishDis = "block";
                this.refreshDis = "none";
            }
        },
        change() {
            if(this.i < this.bigData.length) {
                this.assignValues();
                this.updateButton();
                this.stopCounting();
                this.startCounting();
            } else {
                this.stopCounting();
                this.getButton('finish');
            }
        },
        getButton(button) {
            console.log(button);
            if(button === "previous") {
                this.i--;
                console.log(this.i);
                this.change();
            } else if(button === "next") {
                this.i++;
                console.log(this.i);
                this.change();
            } else if(button === "skip") {
                this.i++;
                console.log(this.i);
                this.change();
            } else if(button === "finish") {
                this.mainEl.innerText = `<span class="scoresheet">you got ${this.correctAns} out of ${this.bigData.length} questions</span>`;
                this.btnCntJus = "center";
                this.prevDis = "none";
                this.nextDis = "none";
                this.skipDis = "none";
                this.finishDis = "none";
                this.refreshDis = "block";
                this.stopCounting();
            } else if(button === "refresh") {
                window.location.reload();
            }
        }  
    },
    created() {
        //this.assignOptionsValues()
        console.log('created app');
    },
    mounted() {
        console.log('mounted app');
        this.assignValues();
        this.startCounting();
        //this.updateButton(); 
        //this.assignOptionColors();
        console.log(this.i);
    },
    updated() {
        console.log("updateded index");
        //this.refreshIndex();
    }
}
</script>

<style scoped>
main {
    width: 100%;
}
/* QUIZ HEADER STYLING */
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
.quiz_cont {
    width: 100%;
    margin: 0;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
/* PROGRESS BAR STYLING */
.progress {
    width: 100%;
    height: 15px;
    background: blue;
}
.moving_color {
    background: red;
}
/* QUESTIONS STYLING */
.q_screen {
    width: 100%;
    height: 100px;
    padding: 5%;
    color: maroon;
    background: white;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 19px;
    font-family: Verdana, Geneva, Tahoma, sans-serif;
}
/* OPTIONS STYLING */
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
/* BUTTONS STYLING */
.btn_cnt {
    width: 90%;
    margin: 0 5%;
    padding: 0;
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    align-items: center;
}
button, button:hover {
    border: none;
    outline: none;
}
.btn_cnt button {
    width: 100px;
    padding: 6px 0;
    margin: 0;
    font-size: 17px;
    color: beige;
    background: rgba(0,0,0,0.8);
    border-radius: 45px;
    cursor: pointer;
    transition: .5s ease-in-out;
}
.btn_cnt button:hover {
    background: rgb(133, 0, 0);
}
</style>