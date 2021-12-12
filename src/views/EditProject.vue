<template>
    <h1>Edit Project</h1>
        <form @submit.prevent="editProject">
            <label>Project Title</label>
            <input type="text" v-model="title">
            <label>Project Detail</label>
            <input type="text" v-model="detail">
            <button @click="updateProject">Update Project</button>
        </form>
</template>

<script>
export default {
    props: ['id'],
    data(){
        return{
            title: "",
            detail: "",
            api: "http://localhost:3000/projects/"
        }
    },
    methods:{
        updateProject(){
            fetch(this.api + this.id, {
                method: "PATCH",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify({
                    title: this.title,
                    detail: this.detail,
                })
            })
            .then(()=>{
                this.$router.push('/');
            })
            .catch(err => console.log(err));
        }
    },
    mounted(){
        fetch(this.api + this.id)
        .then(response => response.json())
        .then(data => {
            this.title = data.title,
            this.detail = data.detail
        })
        .catch(err=>{
            console.log(err);
        })
    }
}
</script>

<style>
    form {
        background: white;
        padding: 20px;
        border-radius: 10px;
    }
    label {
        display: block;
        color: #bbb;
        text-transform: uppercase;
        font-size: 14px;
        font-weight: bold;
        letter-spacing: 1px;
        margin: 20px 0 10px 0
    }
    input {
        padding: 10px;
        border: 0;
        border-bottom: 1px solid #ddd;
        width: 100%;
        box-sizing: border-box;
    }
    textarea {
        border: 1px solid #ddd;
        padding: 10px;
        width: 100%;
        box-sizing: border-box;
        height: 100px;
    }
    form button {
        display: block;
        margin: 20px auto 0;
        background: #00ce89;
        color: white;
        padding: 10px;
        border: 0;
        border-radius: 6px;
        font-size: 16px;
    }
</style>