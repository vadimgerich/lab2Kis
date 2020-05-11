//форма для запитів
<template>
    <form @submit.prevent>
        Дітей менше за <input type="number" v-model.number="query.countOfChilds"> 
        та посада <input type="text" v-model.text="query.posada">
        <input type="button" value="Шукати" @click="search()">
        <input type="button" value="Очистити" @click="reset()">
    </form>
</template>
<script>
export default {
    props:{
        value:Object
    },
    data(){
        return{
            query:this.value
        }
    },
    methods:{
        reset(){
            this.query.posada = this.query.countOfChilds = null;
            this.$emit("input",this.query);
        },
        search(){            
            this.$emit("input",this.query);
        }
    }
}
</script>