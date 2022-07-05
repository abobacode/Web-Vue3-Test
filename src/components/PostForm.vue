<template>
<form @submit.prevent>
            <h4>Создание поста</h4>
            <my-input
                v-model="post.title"
                type="text" 
                placeholder="Название"
            >
            </my-input>         
            <my-input
                v-model="post.body"
                type="text" 
                placeholder="Описание"
            ></my-input>
            <my-button 
                
                style="align-self: flex-end; margin-top: 15px"
                @click="createPost"
            >
                Создать пост
            </my-button>
        </form>
</template>

<script>
    export default {
        data () {
            return {
             post: {
                title: '',
                body: ''
             }   
            }
        },
        methods: {
          createPost() {
            this.post.id = Date.now();
            this.$emit('create', this.post, 'second param', '3 param');
            this.post = {
                title: '',
                body: ''
            } 
          }  
        },
    }
</script>

<style lang="scss" scoped>
form {
    display: flex;
    flex-direction: column;
}
.input {
    width: 100%;
    border: 6px solid rgba(49, 60, 77, 0.363);
    padding: 10px 15px;
    margin-top: 15px;
}
</style>