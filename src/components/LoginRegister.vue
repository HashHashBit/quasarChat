<template>
    <form @submit.prevent="submitForm">
        <q-input
        v-if="tab == 'register'"
        v-model="formData.name"
        class="q-mb-md"
        outlined
        label="Name" />
        <q-input
        v-model="formData.email"
        class="q-mb-md"
        outlined
        type="email"
        label="Email" />
        <q-input
        v-model="formData.password"
        class="q-mb-md"
        outlined
        type="password"
        label="Password" />
        <div class="row">
            <q-space />
            <q-btn
                type="submit"
                color="primary"
                :label="tab" />
        </div>
    </form>
</template>

<script>
    import {mapActions} from 'vuex'
    export default {
        props: ['tab'],
        data() {
            return {
                formData: {
                    name: '',
                    email: 'Jim@gmail.com',
                    password: 'pass123'
                }
            }
        },
        methods: {
            ...mapActions('chatStore', ['registerUser', 'loginUser']),
            submitForm(){
                if (this.tab == 'login') {
                    this.loginUser(this.formData)
                }
                else {
                    this.registerUser(this.formData)
                }
            }
        }
    }
</script>