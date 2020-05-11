
<template>
    <div>
        <p v-if="bugaltList.length==0" class="alert">
            Працівники відсутні
        </p>
        
        <table v-if="bugaltList.length>0">
            <tr>
                <th>#</th>
                <th v-on:click="sort('pib')">  Піб </th>
                <th v-on:click="sort('posada')"> Посада  </th>
                <th v-on:click="sort('payment')">  Плата </th>
                <th v-on:click="sort('countOfChilds')"> Кількість дітей </th>
                <th v-on:click="sort('stage')"> Стаж </th>
                <th></th>
            </tr>
            <bugaltTableRow v-for="(bugalt,index) in bugaltList" 
                v-bind:key="bugalt._id" 
                v-bind="{bugalt,index}"
                @remove="remove"
                @update="update" 
            >             
            </bugaltTableRow>
        </table>
    </div> 
</template>

<script>
import bugaltTableRow from "./bugaltTableRow";

export default {
    name:"bugaltTable",
    props:{
        bugaltList:Array,
    },
    data(){
        return{
           bugalts: this.bugaltList
        }
    },
    components:{
        bugaltTableRow
    },
    methods:{
        //сортування по деякому полю 
        sort(field){
           this.bugaltList.sort((b1,b2)=> b1[field]>=b2[field]?1:-1);
        },
        //вилучити внигу
        remove(index){
            //генруємо подію, вилучення здійснить батьківський компонент
            this.$emit("remove",index);
        },  
        //оновити книгу
        update(index){
            //генруємо подію, оеовлення здійснить батьківський компонент
            this.$emit("update",index);
        }
    }
}
</script>

<style scoped>
    .alert{
        background: yellow;
        color: crimson;
    }

    table, table td{
        border-collapse: collapse;
        border: 1px solid black;
    }
</style>