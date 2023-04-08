<script>
export default {
    props: {
        todos: {
            type: Array,
            default: []
        }
    },
    setup(props, { emit }) {
        const deleteTODO = (index) => {
            emit('deleteTODO', index)
        }

        const done = (index) => {
            emit('doneTodo', index)
        }

        return { deleteTODO, done }
    },
}
</script>

<template>
    <div style="margin-top: 20px;">
        <ul class="list-group">
            <li v-for="todo, index in todos" :key="todo" class="list-group-item">
                <div class="row">
                    <div class="col">
                        <span v-if="todo.isDone">
                            <del>
                                <span>{{ todo.activity }}</span>
                            </del>
                        </span>
                        <span v-else>{{ todo.activity }}</span>
                    </div>
                    <div class="col-auto">
                        <div class="row gx-2">
                            <div class="col">
                                <button @click="deleteTODO(index)" class="btn btn-danger">Delete</button>
                            </div>
                            <div class="col">
                                <span v-if="todo.isDone">
                                    <button @click="done(index)" class="btn btn-info">Undo</button>
                                </span>
                                <span v-else>
                                    <button @click="done(index)" class="btn btn-info">Done</button>
                                </span>
                            </div>
                        </div>
                    </div>
                </div>
            </li>
        </ul>
    </div>
</template>
