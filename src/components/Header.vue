<template>
    <header>
        <div class="logo">
            <span class="logo-image">
                <img src="@/assets/logo.png" alt="BuyList">
            </span>
            <span class="text">BuyList</span>
        </div>
        <div class="date">{{ week }}, {{ date }}</div>
        <!-- <div class="menu">
            <img src="@/assets/menu.svg" alt="menu">
        </div> -->
    </header>
</template>

<script setup>
import { ref } from 'vue';

const date = ref('');
const week = ref('');

var days = ['пн', 'вт', 'ср', 'чт', 'пт', 'сб', 'вс'];
var d = new Date();
var n = d.getDay();

week.value = days[n]

date.value = new Date().toISOString().slice(0, 10).split('-').reverse().join('.')

</script>

<style lang="scss" scoped>
.date{
    font-family: 'Oswald', sans-serif;
}
header{
    position: sticky;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 0;
    margin-bottom: 20px;
}
.logo{
    .logo-image{
        max-width: 45px;
        display: block;
    }
    display: flex;
    justify-content: space-between;
    align-items: center;
}
</style>