<template>
    <div>
        <div @dblclick="changeReminder(tas.id)" :class="[tas.reminder ? 'reminder' : '', 'task']">

        <h1>{{tas.text}}
            <i @click="onDelete(tas.id)" class="fas fa-times"></i>
          
        </h1>
        <p>{{tas.day}}</p>
 
            
        </div>
    </div>
</template>

<script>

export default {
    name: 'Tas',
    props: {
        tas: Object,
    },
    methods: {
        onDelete(id){
            this.$emit('tas-delete', id)
        },
        changeReminder(id){
            this.$emit('color-changer', id)
        }
    },

}
</script>

<style >
.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}
.task.reminder {
  border-left: 5px solid green;
}
.task h1 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>