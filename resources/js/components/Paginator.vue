<template>
    <div class="container" v-if="shouldPaginate">
        <ul class="pagination">
            <li class="page-item" v-show="perURL">
            <a class="page-link" href="#" aria-label="Previous" @click.prevent="page--">
                <span aria-hidden="true">&laquo; Previous</span>
            </a>
            </li>
            <li class="page-item" v-show="nextURL">
            <a class="page-link" href="#" aria-label="Next" @click.prevent="page++">
                <span aria-hidden="true">Next &raquo;</span>
            </a>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        props:['dataSet'],

        data(){
            return{
                page: 1,
                perURL:false,
                nextURL: false
            };
        },

        computed:{
            shouldPaginate(){
                return !! this.perURL || !! this.nextURL
            }
        },

        watch:{
            dataSet(){
                this.page = this.dataSet.current_page;
                this.perURL = this.dataSet.prev_page_url;
                this.nextURL = this.dataSet.next_page_url;
            },

            page(){
                this.broadcasts().updateUrl();
            }
        },

        methods:{
            broadcasts(){
                this.$emit('updated', this.page);

                return this;
            },

            updateUrl(){
                history.pushState(null, null, '?page=' + this.page);
            }
        }
    }
</script>
