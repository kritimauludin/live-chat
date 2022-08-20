<template>
    <div class="chat container">
        <h2 class="center teal-text">Arahin Chat</h2>
        <div class="card">
            <div class="card-content">
                <ul class="messages">
                    <li v-for="message in messages" :key="message.id">
                        <span class="teal-text">{{ message.nameUser }}</span>
                        <span class="grey-text text-darken-3">{{ message.content }}</span>
                        <span class="grey-text time">{{ message.timestamp }}</span>
                    </li>
                </ul>
            </div>
            <div class="card-action">
                <NewMessage :nameUser="nameUser"/>
            </div>
        </div>
    </div>
</template>

<script>
import NewMessage from '@/components/NewMessage'
import db from '@/firebase/config'
import moment from 'moment'

export default{
    name: 'Chat',
    props: ['nameUser'],
    components: {
        NewMessage
    },
    data(){
        return {
            messages: []
        }
    },
    created(){
        let ref = db.collection('messages').orderBy('timestamp')

        ref.onSnapshot(snapshot => {
            snapshot.docChanges().forEach(change => {
                if(change.type == 'added'){
                    let doc = change.doc
                    this.messages.push({
                        id: doc.id,
                        nameUser: doc.data().nameUser,
                        content: doc.data().content,
                        timestamp: moment(doc.data().timestamp).format('lll')
                    })
                }
            })
        })
    }
}
</script>

<style>
.chat h2{
    font-size: 2.6em;
    margin-bottom: 40px;
}
.chat span{
    font-size: 1.4em;
}
.chat .time{
    display:block;
    font-size: 0.9em;
}
.messages {
    max-height: 260px;
    overflow: auto;
}
.messages::-webkit-scrollbar{
    width: 3px;
}
.messages::-webkit-scrollbar-track{
    background: #ddd;
}
.messages::-webkit-scrollbar-thumb{
    background: #aaa;
}
</style>
