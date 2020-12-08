<template>
    <app-layout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                <chat-room-selection v-if="currentRoom.id" :rooms="chatRooms" :current="currentRoom" v-on:roomchanged="setRoom($event)" />
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg">
                    <message-container :messages="messages" />
                    <input-message :room="currentRoom"
                    v-on:messagesent="getMessages()"
                     />
                </div>
            </div>
        </div>
    </app-layout>
</template>

<script>
    import AppLayout from '@/Layouts/AppLayout'
    import MessageContainer from './messageContainer'
    import InputMessage from './inputMessage'
    import ChatRoomSelection from './chatRoomSelection'

    export default {
        components: {
            AppLayout,
            MessageContainer,
            InputMessage,
            ChatRoomSelection
        },
        data:()=>({
            chatRooms: [],
            currentRoom: [],
            messages :[],
        }),
        watch:{
            currentRoom(val,oldVal){
                if(oldVal.id){
                    this.disconnect(oldVal);
                }
                this.connect();
            }
        },
        methods:{
            connect(){
                if(this.currentRoom.id){
                    let vm = this;
                this.getMessages();
                window.Echo.private("chat." + this.currentRoom.id)
                .listen('.message.new',e =>{
                    vm.getMessages();
                })
                }
            },
            disconnect(room){
                window.Echo.leave('chat.' + room.id);
            },
            getRooms(){
                axios.get('/chat/rooms').then(res => {
                    this.chatRooms = res.data;
                    this.setRoom(res.data[0]);
                }).catch(err=>{
                    console.log(err);
                })
            },
            setRoom(room){
                this.currentRoom = room;
            },
            getMessages(){
                axios.get('/chat/room/' + this.currentRoom.id + '/messages')
                .then(res =>{
                    if(res.data.length > 0){
                        this.messages = res.data;
                    }
                    else{
                        this.messages = [];
                    }
                    
                })
                .catch(err => {
                    console.log(err);
                })
            }
        },
        created(){
            this.getRooms();
        }
    }
</script>
