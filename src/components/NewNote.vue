<template>
    <form class="new-note" @submit.prevent="addNote">
        <div class="new-note__flex">
            <div class="new-note__item new-note__item_title">
                <label>Title</label>
                <input v-model="note.title" type="text">
            </div>
            <div class="new-note__item">
                <label>Priority</label>
                <select v-model="note.priority">
                    <option v-for="(item, index) in priority" 
                            :key="index" 
                            :value="item.alias">
                            {{ item.title }}
                    </option>
                </select>
            </div>
        </div>
        <label>Description</label>
        <textarea v-model="note.descr"></textarea>
        <button class="btn btnPrimary">New Note</button>
    </form>
</template>

<script>
export default {
    props:{
        note: {
            type: Object,
            required: true
        },
        priority: {
            type: Array,
            required: true
        }
    },
    methods: {
        addNote(){
            this.$emit('addNote', this.note)
        }
    }
}
</script>

<style lang="scss">
.new-note{
    text-align: center;
    margin-bottom: 40px;
    .btn{
        margin-top: 20px;
    }
    &__flex{
        display: flex;
        justify-content: space-between;
    }
    &__item{
        width: 40%;
        &_title{
            width: 55%;
        }
    }
}
</style>