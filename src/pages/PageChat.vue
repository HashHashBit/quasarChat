<template>
    <q-page
    id="messagesCard"
    ref="pageChat"
    class="flex column">
        <q-banner
        v-if="!otherUserDetails.online"
        class="bg-grey-4 text-center">
            {{ otherUserDetails.name }} User is offline
        </q-banner>
        <div class="q-pa-md column col justify-end">
            <q-chat-message
                v-for="(message, key) in messages"
                :key="key"
                :name="message.from == 'me' ? userDetails.name : otherUserDetails.name"
                :text="[message.text]"
                :sent="message.from == 'me' ? true : false"
            />
        </div>
        <q-footer elevated>
            <q-toolbar>
                <q-form 
                    @submit="sendMessage"
                    class="full-width"
                >
                    <q-input
                        v-model="newMessage"
                        bg-color="white"
                        outlined
                        rounded
                        label="Message"
                        dense
                    >
                    <template v-slot:after>
                        <q-btn
                            round
                            dense
                            flat
                            @click="sendMessage"
                            color="white"
                            icon="send"
                        />
                    </template>
                    </q-input>
                </q-form>
            </q-toolbar>
        </q-footer>
    </q-page>
</template>

<script>
import { mapState, mapActions} from "vuex"
import mixinOtherUserDetails from 'src/mixins/mixinOtherUserDetails.js'
export default {
    mixins: [mixinOtherUserDetails],
    data() {
        return {
            newMessage: '',
        }
    },

    computed: {
        ...mapState('chatStore', ['messages', 'userDetails']),
    },

    updated() {
        this.$nextTick(() => this.scrollToEnd());
    },

    methods: {
        ...mapActions('chatStore', ['firebaseGetMessages', 'firebaseStopGettingMessages', 'firebaseSendMessage']),
        sendMessage() {
            this.firebaseSendMessage({
                message: {
                    text: this.newMessage,
                    from: 'me'
                },
                otherUserId: this.$route.params.otherUserId
            })
        },
    },
    watch: {

    },
    mounted() { 
        this.firebaseGetMessages(this.$route.params.otherUserId)
        window.scrollTo(0, document.body.scrollHeight || document.documentElement.scrollHeight);
    },
    unmounted() {
        this.firebaseStopGettingMessages()
    }
}
</script>

<style>
</style>