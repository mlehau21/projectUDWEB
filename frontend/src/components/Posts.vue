<template>
    <div>
        <div class="container-fluid">
            <div class="row">
                <div class="col-xs-6 col-lg-3" v-for="post in posts" :key="post._id">    
                    <h4><b>{{ post.title }}</b></h4> 
                    <span>{{ post.description }}</span>
                    <div class="right">
                        <router-link tag="button" class="btn btn-primary" :to="{ name: 'postEdit', params: { id: post._id} }">Sửa</router-link>
                        <button class="btn btn-danger" @click="deletePost(post._id)">Xóa</button>
                    </div>
                </div>
            </div>
        </div>
  </div>
</template>

<script>
import axios from '../axios-auth';

export default {
    data() {
        return {
            posts: [],
            id: ''
        }
    },
    created() {
        axios.defaults.headers.common['Authorization'] = localStorage.getItem('jwtToken')
        axios.get('/posts')
        .then(res => {
            this.posts = res.data
        })
        .catch(error => {
            this.$router.push('/login')
        })
    },
    methods: {
        deletePost(id) {
            axios.delete(`/posts/delete/${id}`)
            .then(res => {
                console.log(res)
            })
            this.$router.push({ name: 'posts'})
        }
    }
}
</script>

<style scoped> 
.col-lg-3 {
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
    transition: 0.3s;
    height: 140px;
    margin-top: 50px;
}

.col-lg-3:hover {
    box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2);
}

.right {
    margin: 2%;
}
</style>
