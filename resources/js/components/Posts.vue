<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">

                <h1>Posts</h1>
                <div>
                    <div class="mb-4" v-for="post in posts" :key="post.id">
                        <div class="card" >
                            <div class="card-header"> {{ post.title }}</div>

                            <div class="card-body">{{ post.body }}</div>
                        </div>
                    </div>

                    <paginator :dataSet="dataSet" @updated="fetch"></paginator>

                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Paginator from './Paginator.vue';

    export default {
        components: { Paginator },
        name: 'Posts',

        data() {
            return{
                posts:[],
                dataSet:''
            };
        },
        created() {
            this.fetch();
        },

        methods:{
            fetch(page){
                axios.get(this.url(page)).then(response => {
                    this.posts = response.data.data;
                    this.dataSet = response.data;
                });
            },

            url(page){

                if(! page){
                    let query =  location.search.match(/page=(\d+)/)
                    page = query ? query[1] : 1;
                }
                
                return '/posts?page=' + page;
            }
        }
    }
</script>
