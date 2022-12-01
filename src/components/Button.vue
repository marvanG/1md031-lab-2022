<template>
        <button @click="onClick()">
            <img src="send_button_icon.png" style="width: 20px" >
             {{text}}
            
        </button>
        

    

   
        

</template>

<script>
export default {
    name: 'Button',
    props: {
        text: String,

    },
    methods: {
        onClick() {
            console.log('klickad')
            this.$emit('place-order')
        }
    }
}

</script>

<style scoped>
Button {
    margin-left: 10px;
    
   

}
</style>