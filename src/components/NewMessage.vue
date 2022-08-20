<template>
    <div class="new-message">
        <form @submit.prevent="addMessage">
            <label for="new-message">New Message (enter to send chat) :</label>
            <input type="text" name="new-message" v-model="newMessage">
            <p class="red-text">{{ feedback }}</p>
        </form>
    </div>
</template>

<script>
import db from  '@/firebase/config'
export default{
    name: 'NewMessage',
    props: ['nameUser'],
    data(){
        return{
            newMessage: null,
            feedback: null,
        }
    },
    methods: {
        addMessage(){
           if(this.newMessage){
                db.collection('messages').add({
                    content : this.newMessage,
                    nameUser: this.nameUser,
                    timestamp: Date.now()
                }).catch(err => {
                    console.log(err)
                })
                this.newMessage = null
                this.feedback = null
           }else{
                this.feedback = "Please insert your message!!"
           }
        }
    }
}
</script>