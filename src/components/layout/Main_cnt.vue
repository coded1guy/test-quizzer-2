<template>
    <main id="main">
		<!--HEADER HTML-->
        <header>
            <span class="category">
                <b>category:</b> {{ category }}
            </span>
            <span class="atmpt_qstn">
                <b>Attempted Questions:</b> {{ ids }}
            </span>
            <span class="timer">
                <b>Timer:</b> {{ counter }}
            </span>
        </header>
		<div class="scoreBoard" :style="{ display: uncontStat }">
			<span>You got {{ score }} out of {{ this.bigData.length }} questions</span>
			<button @click="getButton('refresh')">start again</button>
		</div>
        <div class="quiz_cont" :style="{ display: contStat }">
            <!--PROGRESS HTML-->
            <div class="progress" ref="progress">
				<span :style="{ width: pWidth }"></span>
			</div>
            <!--QUESTIONS HTML-->
            <div class="q_screen">
                {{ qPane }}
            </div>
            <!--OPTIONS HTML-->
            <div class="op_screen">
                <div class="optn" :style="{ background: op1Bg }" @click="validate(optOne, 'A')" v-on:click="progress">
                    <span>A.</span>
                    <div>
                        <span id="op1_check">{{ optOne }}</span>
                    </div>
                </div>
                <div class="optn" :style="{ background: op2Bg }" @click="validate(optTwo, 'B')">
                    <span>B.</span>
                    <div>
                        <span id="op2_check">{{ optTwo }}</span>
                    </div>
                </div>
                <div class="optn" :style="{ background: op3Bg }" @click="validate(optThree, 'C')">
                    <span>C.</span>
                    <div>
                        <span id="op3_check">{{ optThree }}</span>
                    </div>
                </div>
            </div>
            <!--BUTTONS HTML-->
            <div class="btn_cnt">
                <button :style ="{ display: prevDis }" @click="getButton('previous')">previous</button>
                <button :style ="{ display: nextDis }" @click="getButton('next')">next</button>
                <button :style ="{ display: skipDis }" @click="getButton('skip')">skip</button>
                <button :style ="{ display: finishDis }" @click="getButton('finish')">finish</button>
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
			uncontStat: "none",
            contStat: "flex",
            score: 0,
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
            // STYLES DATA
            prevDis: "block",
            nextDis: "block",
            skipDis: "block",
            finishDis: "block",
            op1Bg: "rgb(163, 163, 68)",
            op2Bg: "rgb(163, 163, 68)",
			op3Bg: "rgb(163, 163, 68)",
			pWidth: ''
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

			this.progress();
        },
        startCounting() {
            this.counter = 10;
            this.currentCount = 10;
            this.timer = setInterval(() => {
                this.currentCount--;
                this.counter = this.currentCount;
                if(this.i < this.bigData.length && this.currentCount === 0) {
                    this.getButton('skip');
                } else if(this.i === this.bigData.length && this.currentCount === 0) {
                    this.getButton('finish');
                }
            }, 1000);
        },
        stopCounting() {
            clearInterval(this.timer);
            this.counter = 10;
            this.currentCount = 10;
        },
        // OPTIONS SCRIPT
        validate(option, opChoice) {
			let correctAnswer = this.bigData[this.i].answer;
			console.log();
			if(option === correctAnswer) {
				this.score++;
				if(opChoice === 'A') {
					this.op1Bg = "green";
				} else if(opChoice === 'B') {
					this.op2Bg = "green";
				} else if(opChoice === 'C') {
					this.op3Bg = "green";
				}
				setTimeout(() => {
					this.op1Bg = "rgb(163, 163, 68)";
					this.op2Bg = "rgb(163, 163, 68)";
					this.op3Bg = "rgb(163, 163, 68)";
					this.getButton("next");
				}, 500);
			} else {
				if(opChoice === 'A') {
					this.op1Bg = "red";
				} else if(opChoice === 'B') {
					this.op2Bg = "red";
				} else if(opChoice === 'C') {
					this.op3Bg = "red";
				}
				setTimeout(() => {
					this.op1Bg = "rgb(163, 163, 68)";
					this.op2Bg = "rgb(163, 163, 68)";
					this.op3Bg = "rgb(163, 163, 68)";
					this.getButton("next");
				}, 500);
			}
		},
        progress() {
			let pCont = this.$refs.progress;
			let lone = this.bigData.length;
			let d = ((this.i + 1) / lone);
			let a = pCont.offsetWidth;
			let b = a * d;
			this.pWidth = `${b}px`;
		},
        // BUTTONS SCRIPT
        updateButton() {
            if(this.i === 0) {
                this.prevDis = "none";
                this.nextDis = "block";
                this.skipDis = "block";
                this.finishDis = "none";
                this.refreshDis = "none";
            }
            if (this.i < this.bigData.length && this.i > 0) {
                this.prevDis = "block";
                this.nextDis = "block";
                this.skipDis = "block";
                this.finishDis = "none";
                this.refreshDis = "none";
            }
            if (this.i === (this.bigData.length) - 1) {
                this.prevDis = "block";
                this.nextDis = "none";
                this.skipDis = "none";
                this.finishDis = "block";
                this.refreshDis = "none";
            }
            if(this.i >= this.bigData.length) { 
				this.i = this.bigData.length;
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
            if(button === "previous") {
                this.i--;
                this.change();
            } else if(button === "next") {
                this.i++;
                this.change();
            } else if(button === "skip") {
                this.i++;
                this.change();
            } else if(button === "finish") {
				this.uncontStat = "flex";
				this.contStat = "none";
                this.stopCounting();
            } else if(button === "refresh") {
                window.location.reload();
            }
        }  
    },
    created() {
        //this.assignOptionsValues()
    },
    mounted() {
        this.assignValues();
        this.startCounting()
        this.updateButton();
        //this.updateButton();
    },
    updated() {
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
    font-family: 'Poppins', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-weight: 500;
}
.scoreBoard {
	width: 100%; 
	height: 300px;
	background: rgba(225,225,225, .3);
	flex-direction: column; 
	align-items: center; 
	justify-content: center;
	font-family: 'Montserrat', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-weight: 500;
}
.scoreBoard span {
	font-size: 20px;
	font-weight: 600;
}
.scoreBoard button {
	width: 130px;
    padding: 6px 0;
    margin: 20px 0 0 0;
    font-size: 17px;
    color: rgb(245, 245, 220);
    background: rgba(0,0,0,0.8);
    border-radius: 45px;
    cursor: pointer;
    transition: .5s ease-in-out;
}
.scoreBoard button:hover {
    background: rgb(133, 0, 0);
}
.quiz_cont {
    width: 100%;
    margin: 0;
    flex-direction: column;
    justify-content: center;
    align-items: center;
	font-family: 'Montserrat', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-weight: 500;
}
/* PROGRESS BAR STYLING */
.progress {
    width: 100%;
    height: 15px;
	display: flex;
	flex-direction: row;
	flex-wrap: no-wrap;
	justify-content: flex-start;
    background: rgb(245, 245, 220);
	padding: 0;
}
.progress span {
    background: rgb(228, 228, 100);
	margin: 0;
	padding: 0;
	height: 15px;
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
	font-family: 'Montserrat', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-weight: 500;
}
/* OPTIONS STYLING */
.op_screen {
    width: 100%;
    display: flex;
    flex-direction: column;
    padding: 3% 0;
    font-size: 17px;
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
    transition: .3s ease-in-out;
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
	justify-content: space-between;
    align-items: center;
}
button, button:hover {
    border: none;
    outline: none;
	font-family: 'Montserrat', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-weight: 500;
}
.btn_cnt button {
    width: 110px;
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