<template>
    <main>
        <quizHeader v-bind:qHeader="bigData" v-bind:index="i" v-on:clicked-button="getButton"/>
        <div class="quiz_cont">
            <div class="progress">
                <span class="moving_color"></span>
            </div>
            <questionPanel v-bind:qQuestion="bigData" v-bind:index="i"/>
            <options v-bind:qOptions="bigData" v-bind:index="i" v-on:chosen-option="validate"/>
            <quizzerButtons v-bind:qButtons="bigData" v-bind:index="i" v-on:clicked-button="getButton"/>
        </div>
    </main>
</template>

<script>
import options from './options';
import quizHeader from './quiz_header';
import quizzerButtons from './quizzer_buttons';
import questionPanel from './question_panel';

export default {
    name: "MainCnt",
    props: ["bigData"],
    components: {
        quizHeader, options, questionPanel, quizzerButtons
    },
    data() {
        return {
            GEvent: '',
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
            
        }
    },
    methods: {
        refreshIndex() {
            console.log(this.$GEvent + " from maincnt");
            //window.$emit('refresh-index')
        },
        validate(optOne) {
            console.log(optOne);
        },
        getButton(button) {
            console.log(button);
            if(button === "previous") {
                this.i--;
                console.log(this.i);
            } else if(button === "next") {
                this.i++;
                console.log(this.i);
            } else if(button === "skip") {
                this.i++;
                console.log(this.i);
            } else if(button === "refresh") {
                window.location.reload();
                console.log(this.i);
            }
        },
        restoreStates() {
            // prevBtn.style.display = "inline-block";
            // prevBtn.disabled = true;
            // finishBtn.style.display = "none";
            // refreshBtn.style.display = "none";
            // assignValues();   
        },  
    },
    created() {
        
    },
    mounted() {
        //
    },
    updated() {
        console.log("updateded index");
        this.refreshIndex();
    }
}
</script>

<style scoped>
main {
    width: 100%;
}
.quiz_cont {
    width: 100%;
    margin: 0;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
.progress {
    width: 100%;
    height: 15px;
    background: blue;
}
.moving_color {
    background: red;
}
</style>