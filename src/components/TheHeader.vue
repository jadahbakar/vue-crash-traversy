<template>
    <header>
        <h1>
            {{title }}
        </h1>
        <BaseButton 
        @btnClick="$emit('toggle-add-task')" 
        :text="showIt ? 'Close': 'Add Task'" 
        :color="showIt ? 'red' : 'green'"/>
    </header>
</template>


<script>
import BaseButton from './BaseButton'

export default {
    name: 'TheHeader',
    // props: ['title']
    props: {
        title: String,
        showIt: Boolean,
    },
    components: { BaseButton }
}
</script>

<style scoped>
    header{
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>