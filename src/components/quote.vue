<template>
    <div
        :style="{
        'background': 'url('+ bgImage +')'
        }"
         class="d-flex h-100 w-100 bgImage"
    >
        <div class="align-self-center w-100">
            <div class="col-12 mx-auto">
                <div>
                    <h1 class="display-3 font-weight-bold">
                        <span class="time">{{hh}}:{{mm}}</span>
                    </h1>
                    <h2 class="name">{{greetings}},{{userNameProp}}!</h2>
                </div>

            </div>
            <p class="quote-text lead col-12 mx-auto font-weight-bold" :style="[quote === 'problem loading quote' ? errorStyle : '']">
                {{quote}}
            </p>
        </div>
        <username-dialog :userNameProp="userNameProp" @update="updateUserName"></username-dialog>
    </div>
</template>

<script>
    import UsernameDialog from "./usernameDialog";
    const imagePlaceholder = "https://cdn.pixabay.com/photo/2015/12/01/20/28/road-1072823_960_720.jpg";
    export default {
        name: "quote",
        components: {UsernameDialog},
        data(){
            return {
                bgImage: imagePlaceholder,
                quote: '',
                greetings: '',
                hh: '00',
                mm: '00',
                errorStyle: {
                    color: 'red !important',
                },
                userNameProp: '',

            }
        },

        mounted() {
            this.fetchNewImage();
            setInterval(this.fetchNewImage, 1800000);
            this.startClock();
            this.fetchNewQuote();
            setInterval(this.fetchNewQuote, 300000);
        },
        methods: {
            updateUserName(userNameProp){
                this.userNameProp=userNameProp;
            },
            fetchNewQuote() {
                this.quote = 'loading...';
                fetch("https://quotes.rest/quote/random?language=en&limit=1", {
                    headers: {
                        Accept: "application/json",
                        "X-Theysaidso-Api-Secret": ""
                    }
                }).then((data) => {
                    this.quote = data.contents.quotes.quote;
                }).catch(() => {
                    this.quote = "problem loading quote";
                })
            },
            fetchNewImage() {
                fetch('https://lorempixel.com/640/480/nature', {
                    mode: 'no-cors',
                }).then(() => {
                    this.bgImage = 'https://lorempixel.com/640/480/nature'
                }).catch(() => {
                    this.bgImage = imagePlaceholder
                })
            },
            startClock() {
                this.newTime();
                setInterval(this.newTime, 1000);
            },

            newTime() {
                let myDate = new Date();
                this.hh = myDate.getHours() < 10 ? '0' + myDate.getHours() : myDate.getHours();
                this.mm = myDate.getMinutes() < 10 ? '0' + myDate.getMinutes() : myDate.getMinutes();
                this.setGreetings(myDate.getHours())
            },
            setGreetings(hrs) {
                if (hrs < 12)
                    this.greetings = 'Good Morning';
                else if (hrs >= 12 && hrs <= 17)
                    this.greetings = 'Good Afternoon';
                else if (hrs >= 17 && hrs <= 24)
                    this.greetings = 'Good Evening';
            },
        }
    }
</script>

<style scoped>
    .bgImage{
        background-repeat: no-repeat !important;
        background-size: cover !important;
    }
    h1, h2, p{
        color: white;

    }

    .time{
        font-size: 70px !important;
        text-shadow: 3px 3px 0 #000, -2px -1px 0 #000, 0px 0px 0 #000, 0px -1px 0 #000, 2px 0px 0 #000;
    }

    .greetings{
        text-shadow: 3px 3px 0 #000, -2px -1px 0 #000, 0px 0px 0 #000, 0px -1px 0 #000, 2px 0px 0 #000;
    }

    .quote-text{
        text-shadow: 1px 1px 0 #000, 0px 0px 0 #000, 0px 0px 0 #000, 0px 0px 0 #000, 1px 0px 0 #000;
        position: fixed !important;
        bottom: 0;
    }

    .name{
        text-shadow: 1px 1px 0 #000, 0px 0px 0 #000, 0px 0px 0 #000, 0px 0px 0 #000, 2px 0px 0 #000;
        text-transform: capitalize;
    }
</style>