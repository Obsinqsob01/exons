<template>
    <div>
        <div class="notification is-danger" v-if="error">
            {{ error }}            
        </div>
        <h1 class="title is-1 has-text-centered">
            Agregar una comunidad
        </h1>
        <div class="field">
            <input type="text" v-model="title" class="input" placeholder="title">
        </div>
        <div class="fiel">
            <textarea v-model="description" class="textarea" placeholder="description"></textarea>
        </div>
        <br>
        <button @click="onClick()" class="button is-info">Subir</button>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'Addexonsunity',
    data() {
        return {
            title: "",
            description: "",
            error: ""
        }
    },
    beforeMount() {
        if (!localStorage.getItem("user")) {
            this.$router.push('/');
        }
    },
    methods: {
        onClick() {
            var ls = localStorage.getItem('token');
            ls = JSON.parse(ls);

            let config = {
                headers: {
                    Authorization: `Bearer ${ls.token}`
                }
            };

            axios.post('http://localhost:8080/api/community/add', {
                title: this.title,
                description: this.description
            }, config).then(res => {
                this.$router.push('/');
            }).catch(err => {
                console.log(err.response);
                this.error = "Ha ocurrido un error, intentalo después :("
            })
        }
    }
}
</script>
