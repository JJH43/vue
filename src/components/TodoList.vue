<template>
    <div class="inputBox shadow">
        <TransitionGroup name="list" tag="ul">
            <ul>
                <li v-for="(todo, index) in props.todoArr" :key="index" class="shadow">
                    <i class="fas fa-check checkBtn" :class="{ checkBtnCompleted: todo.completed }"
                    @click="toggleComplete(todo, index)"></i>
                    <span :class="{ textCompleted: todo.completed }">{{ todo.item }}</span>
                    <span class="removeBtn" @click="removeTodo(todo.item, index)">
                        <i class="fas fa-trash-alt"></i>
                    </span>
                </li>
            </ul>
        </TransitionGroup>
    </div>
</template>

<script setup>

const props = defineProps(['todoArr'])

const emit = defineEmits(['remove:todo', 'toggle:todo'])

const removeTodo = (todoItem, index) => {
  emit('remove:todo', todoItem, index)
}

const toggleComplete = (todoItem, index) => {
  emit('toggle:todo', todoItem, index)
}

</script>

<style scoped>
.list-enter-active, .list-leave-active {
    transition: all 0.5s ease;
}
.list-enter-from, .list-leave-to {
    opacity: 0;
    transform: translateX(30px);
}

i,
span {
    cursor: pointer;
}

ul {
    list-style-type: none;
    padding-left: 0px;
    margin-top: 0;
    text-align: left;
}

li {
    display: flex;
    min-height: 50px;
    height: 50px;
    line-height: 50px;
    margin: 0.5rem 0;
    padding: 0 0.9rem;
    background: white;
    border-radius: 5px;
}

.removeBtn {
    margin-left: auto;
    color: #de4343;
}

.checkBtn {
    line-height: 45px;
    color: #62acde;
    margin-right: 5px;
}

.checkBtnCompleted {
    color: #b3adad;
}

.textCompleted {
    text-decoration: line-through;
    color: #b3adad;
}
</style>
