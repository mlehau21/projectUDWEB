<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12 col-lg-8 col-lg-offset-2">
                <h2>ĐĂNG KÝ</h2>
                <form @submit.prevent="onSubmit">
                    <div class="form-group" :class="{invalid: $v.email.$error}">
                        <label for="email">Email:</label>
                        <input type="email" class="form-control" id="email" placeholder="Nhập email" name="email" @blur="$v.email.$touch()" v-model="email">
                        <p v-if="!$v.email.email">Nhập email của bạn</p>
                        <p v-if="!$v.email.required">Đây là thông tin bắt buộc</p>
                    </div>

                    <div class="form-group" :class="{invalid: $v.password.$error}">
                        <label for="pwd">Mật khẩu:</label>
                        <input type="password" class="form-control" id="pwd" placeholder="Nhập mật khẩu" name="pwd" @blur="$v.password.$touch()" v-model="password">
                    </div>
    
                    <div class="form-group" :class="{invalid: $v.confirmPassword.$error}">
                        <label for="pwd1">Nhập lại mật khẩu:</label>
                        <input type="password" class="form-control" id="pwd1" placeholder="Nhập lại mật khẩu" name="pwd" @blur="$v.confirmPassword.$touch()" v-model="confirmPassword">
                        <p v-if="$v.confirmPassword.$error">Mật khẩu không khớp</p>
                    </div>

                    <button type="submit" :disabled="$v.$invalid">Xác nhận</button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
    import { required, email, minValue, minLength, sameAs } from 'vuelidate/lib/validators'
    import axios from '../axios-auth.js'

    export default {
        data() {
            return {
                email: '',
                password: '',
                confirmPassword: '',
                message: ''
            }
        },
        validations: {
            email: {
                required,
                email
            },
            password: {
                required,
                minLen: minLength(6)
            },
            confirmPassword: {
                sameAs: sameAs(vm => {
                    return vm.password
                })
            }
        },
        methods: {
            onSubmit() {
                const formData = {
                    email: this.email,
                    password: this.password
                }
                axios.post('/auth/register', formData)
                .then(res => {
                    if(res.data.success) {
                        this.$router.push('/login')
                    } else {
                        console.log(res.data)
                    }
                })
                .catch(error => console.log(error))
            }
        }
    }
</script>

<style scoped>
    .col-lg-8 {
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
    transition: 0.3s;
    height: auto;
    margin-top: 50px;
    padding: 2vh;
}

div.invalid input {
    border: 1px solid red;
    background-color: #ffc9aa;  
}
div.invalid label {
    color: red;
}
</style>
