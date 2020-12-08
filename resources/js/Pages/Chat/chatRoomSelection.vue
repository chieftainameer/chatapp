<template>
    <div class="grid grid-cols-2">
        
        <div class="font-bold text-xl">
            {{selected.name}} Chat
        </div>
        <div>
            <select v-model="selected" class="float-right" @change="$emit('roomchanged',selected)" >
                <option v-for="(room,index) in rooms" :key="index" :value="room" >{{room.name}}</option>
            </select>
        </div>
    </div>
</template>
<script>
export default {
    props:['rooms','current'],
    data:()=>({
        selected:'',
    }),
    created(){
        this.selected = this.current;
        console.log('selection ' + this.current);
    }
}
</script>
<style scoped>

</style>