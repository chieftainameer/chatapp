<template>
    <!-- <div>input message {{this.room.name}}</div> -->
    <div>
        <input type="text" v-model="message" @keyup.enter="sendMessage" />
        <button @click="sendMessage" >Send</button>
    </div>
</template>
<script>
export default {
    props:['room'],
    data:()=>({
        message:'',
    }),
    methods:{
        sendMessage(){
            if(this.message == ' '){
                return;
            }
            axios.post('/chat/room/' + this.room.id + '/message',{message:this.message})
            .then(res => {
                if(res.status == 201){
                    this.message = ' ';
                    this.$emit('messagesent');
                }
            })
            .catch(err => {
                console.log("Message could not be sent try again later");
            })
        }
    }
}
</script>
<style scoped>

</style>