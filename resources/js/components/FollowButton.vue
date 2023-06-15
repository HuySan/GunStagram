<template>
    <div>
        <button class="btn btn-primary ml-4" @click="followUser" v-text="buttonText"></button>
    </div>
</template>

<script>
    export default {
        //Это свойство указывает, что компонент ожидает, что при использовании компонента будет передано свойство с именем userId
        props: ['userId', 'follows'],

        mounted() {
            console.log('Component mounted.')
        },

        data: function(){
            return{
                status: this.follows,
            }
        },

        methods: {
            followUser () {
                //axios - популярная библиотека для исполнения post запросов
                axios.post('/follow/' + this.userId)
                    .then(response => {
                        this.status = ! this.status;//при успешном ответе просто "переворачиваем стату подписи - unfollow - follow"

                        console.log(response.data);
                    })
                    .catch(errors=>{
                        if(errors.response.status == 401){
                            window.location = '/login';
                        }
                    });
            }
        },

        computed: {
            buttonText(){
                return (this.status) ? 'Unfollow' : 'Follow';
            }
        },
    }
</script>
