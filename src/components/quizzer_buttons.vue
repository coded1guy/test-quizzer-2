<template>
    <div class="btn_cnt">
        <button id="prev" style="display: none;" @click="$emit('clicked-button', 'previous')" v-on:click="see">previous</button>
        <button id="nxt" @click="$emit('clicked-button', 'next')" v-on:click="see">next</button>
        <button id="skip" @click="$emit('clicked-button', 'skip')" v-on:click="see">skip</button>
        <button id="finish" @click="$emit('clicked-button', 'finish')">finish</button>
        <button id="refresh" @click="$emit('clicked-button', 'refresh')">start again</button>
    </div>
</template>

<script>
export default {
    name: "quizzerButtons",
    props: ["qButtons", "index"],
    data() {
        return {
            prevBtn: document.querySelector('.prev'),
            nextBtn: document.querySelector('.nxt'),
            skipBtn: document.querySelector('.skip'),
            finishBtn: document.querySelector('.finish'),
            refreshBtn: document.querySelector('.refresh')
        }
    },
    method: {
        see(){
            console.log(this.qButtons);
        }
    }
}
</script>

<style scoped>
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