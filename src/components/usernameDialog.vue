<template>
    <div>
        <b-modal
                :hide-header-close="true"
                :no-close-on-backdrop="true"
                v-model="modalShow"
                title="Type your name to continue"
                ok-only
                hide-footer
                aria-describedby="input-error-msg"
        >
            <b-form-input
                    v-model="userNameModal"
                    :state="userNameError"
                    placeholder="first name">
            </b-form-input>
            <b-form-invalid-feedback id="input-error-msg">
                please provide your name!
            </b-form-invalid-feedback>
            <b-button
                    class="mt-3"
                    @click="saveUserName()"
            >Save</b-button>
        </b-modal>
    </div>
</template>

<script>
    export default {
        name: "usernameDialog",
        data(){
            return{
                modalShow:false,
                userNameModal: ''
            }
        },
        props: {
            userNameProp: {
                default: '',
                type: String,
            },
        },
        mounted() {
            this.modalShow = true;
        },
        computed: {
            userNameError() {
                return this.userNameModal !== '';
            }
        },
        methods:{
            saveUserName(){
                this.$emit('update', this.userNameModal);
                this.modalShow = false;

            }
        }
    }
</script>

<style scoped>

</style>