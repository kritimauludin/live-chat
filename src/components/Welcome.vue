<template>
<div class="welcome container">
    <div class="card">
        <div class="card-content center-align">
            <h2 class="teal-text">Welcome to Arahin Chat</h2>
            <form @submit.prevent="enterToChat">
                <label for="nameUser">Type Your Name : </label>
                <input type="text" name="nameUser" v-model="nameUser">
                <p v-if="feedback" class="red-text">{{ feedback }}</p>
                <button class="btn teal btn-enter">Enter to Chat</button>
            </form>
        </div>
    </div>
</div>
</template>

<script>
export default {
    name: 'Welcome',
    data(){
        return {
            nameUser: null,
            feedback: null
        }
    },
    methods: {
        enterToChat(){
            if(this.nameUser){
                this.$router.push({name: 'Chat', params: { nameUser: this.nameUser }})
            }else{
                this.feedback = 'Please insert your name, For joining the chat!!'
            }
        }
    }
}
</script>

<style>
 .welcome{
   max-width: 400px;
   margin-top: 100px;
 }
 .welcome h2 {
    font-size: 2em;
 }
 .btn-enter{
    margin: 30px auto;
 }
</style>