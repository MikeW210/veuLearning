<template>
    <div :key="tas.id" v-for="tas in task"> 
        <Tas @color-changer="$emit('color-changer', tas.id)" @tas-delete="$emit('tas-delete', tas.id)" :tas="tas" />
    </div>
</template>

<script>
import Tas from './Tas.vue'
export default {
    name: 'Task',
    props: {
        task: Array,
    },
    components:{
    Tas
    },
    emits: ['tas-delete'],
}
</script>