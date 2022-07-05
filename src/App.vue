<template>
    <div class="app">
    <h1>Страница с постами</h1>
    <my-input
        v-model="searchQuery"
        placeholder="Поиск..."   
    />
    <div class="app__btns">
        <my-button 
            @click="showDialog"
            >
            Создать пост
        </my-button>
        <my-select
            v-model="selectedSort"
            :options="sortOptions"
        />
    </div>   
        <my-dialog v-model:show="dialogVisible">
        <post-form
            @create = "createPost"
        />
        </my-dialog>
        <post-list 
            :posts="sortedAndSearchedPosts"
            @remove="removePost"
            v-if="!isPostLoading"
        />
        <div v-else>Идёт загрузка...</div>  
              
    </div>
</template>

<script>
import PostForm from "./components/PostForm.vue"
import PostList from "@/components/PostList.vue"
//import MyDialog from "./components/UI/MyDialog.vue";
import MyButton from "@/components/UI/MyButton.vue";
import axios from 'axios';
import MySelect from '@/components/UI/MySelect.vue';
import MyInput from "./components/UI/MyInput.vue";
//import { url } from "inspector";
export default {
    components: {
    PostList,
    PostForm,
    //MyDialog,
    MyButton,
    MySelect,
    MyInput
},
    data () {
        return {
            posts: [],
            dialogVisible: false,     
            isPostLoading: false,
            selectedSort: '',
            searchQuery: '',
            page: 1,
            limit: 10,
            totalPage: 0,
            sortOptions: [
                {value: 'title', name: 'По названию'},
                {value: 'body', name: 'По содержимому'}
            ]    
        }
    },
    methods: {
        createPost (post) {
            this.posts.push(post);
            this.dialogVisible=false;
        },
        removePost(post) {
            this.posts=this.posts.filter(p => p.id !== post.id)
        },
        showDialog() {
            this.dialogVisible=true;
        },
        async fetchPosts() {
            try {
                this.isPostLoading=true;
                const response=await axios.get('https://jsonplaceholder.typicode.com/posts', {
                    params: {
                        _page: this.page,
                        _limit: this.limit
                    }       
                });
                this.totalPage=Math.ceil(response.headers['x-total-count'] / this.limit);
                this.posts=response.data;              
            }   catch(e) {
                alert('Ошибка!')
            }   finally {
                this.isPostLoading=false;
            }
        }
    },
    mounted() {
        this.fetchPosts();
    },
    computed: {
        sortedPosts () {
            return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
        },
        sortedAndSearchedPosts() {
            return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))   
        }
    },
    watch: {

    }
}
</script>

<style lang="scss">
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
.app {
    padding: 20px;
}
.app__btns {
    margin: 15px 0;
    display: flex;
    justify-content: space-between;
}
</style>
