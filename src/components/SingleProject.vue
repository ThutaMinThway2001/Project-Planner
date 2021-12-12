<template>
    <div class="project" :class="{complete: project.isCompleted}">
        <div class="flexing">
            <div @click="clickToShowDetail">
                <h3 >{{project.title}}</h3>
            </div>
            <div>
                <span class="material-icons" @click="deletingProcess">
                delete
                </span>

                <router-link :to="{name: 'EditProject', params: {id: project.id}}">
                    <span class="material-icons">
                    edit
                    </span>
                </router-link>

                <span class="material-icons" @click="completingProcess">
                done
                </span>
            </div>
        </div>
        <p v-if="showDetail">{{project.detail}}</p>
    </div>
</template>

<script>
export default {
    props: ['project'],
    data(){
        return{
            showDetail : false,
            api : 'http://localhost:3000/projects/'
        }
    },
    methods:{
        clickToShowDetail(){
            return this.showDetail =! this.showDetail;
        },
        deletingProcess(){
            let deleteRoute = this.api + this.project.id;
            fetch(deleteRoute, {method: "DELETE"})
            .then(() => {
                this.$emit('delete', this.project.id);
            })
            .catch(err => console.log(err));
        },
        completingProcess(){
            let completeRoute = this.api + this.project.id;
            fetch(completeRoute, {
                method: "PATCH",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify({
                    isCompleted: !this.project.isCompleted
                })
            })
            .then(()=>{
                this.$emit("complete", this.project.id)
            })
            .catch(err => console.log(err))
        }
    }
}
</script>

<style>
    .project{
        padding:20px;
        background-color: #f2f2f2;
        margin: 10px;
        border-left: 6px solid crimson;
        border-radius: 8px;
    }
    h3{
        cursor: pointer;
        color:indigo;
    }
    .flexing{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    span{
        margin-left:10px;
    }
    span:hover{
        cursor: pointer;
        color:#777;
    }
    .complete{
        border-left-color: green;
    }
</style>