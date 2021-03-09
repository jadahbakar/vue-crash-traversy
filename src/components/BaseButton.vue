<template>
    <button @click="OnClick()"  :style="{background: color}" class="btn">{{text}}</button>
</template>

<script>
export default {
    name: 'BaseButton',
    props: {
        text: String,
        color: String,
    },
    methods:{
        OnClick(){
            // console.log('click')
            this.$emit('btnClick')
        }
    }
}
</script>