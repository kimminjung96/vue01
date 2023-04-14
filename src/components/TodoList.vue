<template>
    <div class="card" v-for="i in todos" :key="i.id">
        <div class="card-body p-2">
            <div class="d-flex">
                <div class="form-check flex-grow-1">
                    <label class="form-check-label" :class="{ tolist: i.complated }">
                        <input v-model="i.complated" type="checkbox" class="form-check-input">
                        {{ i.subject }} </label>
                </div>
                <div><button class="btn btn-danger btn-sm" @click="deleteTodo(data)">삭제</button></div>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    props: {
        tidis: {
            type: Array,
            require: true
        }
    }
    //setup(props, { emit }) {
    /*         const todos = ref([]);
    
    
            const todoStyle = {
                color: 'gray',
                'text-decoration': 'line-through',
    
            }
            const deleteTodo = (data) => {
                console.log(data);
                todos.value.splice(data, 1)
            }
            return { todoStyle, deleteTodo, todos } */

    //}

}
</script>

<style></style>