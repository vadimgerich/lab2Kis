// рядок що відбражає 1 книгу
<template>
    <tr>        
        <td> {{index}} </td>
        <td> {{bugalt.pib}} </td>
        <td> {{bugalt.posada}} </td>
        <td> {{bugalt.payment}} </td>
        <td> {{bugalt.countOfChilds}} </td>
        <td> {{bugalt.stage}} </td>
        <td>
            <input type="button" value="Вилучити" @click="remove(index)">
            <input type="button" value="Редагувати" @click="update(index)">
        </td>        
    </tr>
</template>

<script>
export default {
    name:"bugaltTableRow",
    props:{
        index:Number,
        bugalt:Object        
    },
    data(){   
        return{

        }
    },
    methods:{        
        remove(index){
            this.$emit("remove", index);
        },
  
        update(index){
            this.$emit("update",index);
        }
    }
}
</script>

<style scoped>
    td{
        border: 1px solid black;
    }
</style>