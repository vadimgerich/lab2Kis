// Форма для відображення книги
<template>
    <form v-if="visible" @submit.prevent> <!-- якщо форма видима то показати її і відмінити надсилання запиту за замовчуванням-->
        <label> Назва книги <input type="text" v-model="bugalt.title" required> </label> <br>
        <label> Автори <setInput v-model="bugalt.authors"> </setInput></label> <br>
        <label> Дата випуску <input type="date" v-model="bugalt.published"> </label> <br>
        <label> Кількість сторінок <input type="number" v-model.number="bugalt.pages" min="0"> </label> <br>
        <label> Ціна <input type="number" v-model.number="bugalt.price" min="0" step="0.01"> </label> <br>  
        <input type="button" @click="save()" value="Зберегти">     
    </form>
</template>

<script>
import setInput from "./setInput";

export default {
    name: "bugaltForm",
    props:{
        //передача значення в props через value і генерування події input необхідне для коректної роботи v-model компонента
        value: Object,
        visible: Boolean
    },
    data(){
        return{
            bugalt:this.value,            
        }
    },
    components:{
        setInput
    },
    methods:{
        save(){
            this.$emit('input', this.bugalt);         
        }
    }
}
</script>
<style scoped>
    form{
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        background: white ;
    }
</style>