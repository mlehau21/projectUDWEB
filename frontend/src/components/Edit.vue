<template>
    <div>
        <div class="container-fluid">
       
                    <div class="container">
                        <h2>Sửa bài viết</h2>
                        <form @submit.prevent="updateData">
                            <div class="form-group">
                                <label for="title">Title:</label>
                                <input type="title" class="form-control"
                                                    id="title" 
                                            
                                                    name="title" 
                                                    v-model="title">
                            </div>
                            <div class="form-group">
                                <label for="description">Nội Dung:</label>
                                <textarea class="form-control" 
                                                    id="description" 
                                                    rows="5" 
                                                    v-model="description"></textarea>
                            </div>
                            <button type="submit" class="btn btn-info">Lưu</button>
                        </form>
                    </div>
                
        </div>
    </div>
</template>


<script>
import axios from '../axios-auth';

export default {
    data() {
        return {
            id: this.$route.params.id,
            title: '',
            description: ''
        }
    },
    created() {
        axios.get(`/posts/edit/${this.id}`)
        .then(res => {
            this.title = res.data.title;
            this.description = res.data.description;
        })
    },  
    methods: {
        updateData() {
            const formData = {
                title: this.title,
                description: this.description
            }
            axios.put(`/posts/add/${this.id}`, formData)
            .then(data => {
               console.log("ok")
            })
            this.$router.push({ name: 'posts'})
        }
    }
}
</script>

<style scoped>

</style>
