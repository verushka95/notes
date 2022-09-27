<template>
    <div class="notes">
        <div class="note" 
            :class="[`note_priority-${note.priority}`, getClassFull]"
            :key="note.id"
            v-for="(note, index) in notes">
            <div class="note-header" :class="getClassFull">
                <p class="note__editor">
                    <span class="note__title" 
                        title="Изменить название"
                        @click="editNote(index, 'title')">
                            {{ note.title }}
                    </span>
                    <input 
                        class="input_hide" 
                        type="text" 
                        v-model="note.title"
                        ref="inputTitle"
                        @focus="noteBefore.oldTitle = note.title" 
                        @keyup.enter="editValue($event, note, 'title')" 
                        @keyup.esc="resetValue(note, 'title')"
                        />
                </p>
                <p style="cursor: pointer;" @click="removeNote(note.id)">X</p>
            </div>
            <div class="note-body">
                <p>
                   <span 
                        title="Изменить описание" 
                        class="note__descr" 
                        @click="editNote(index, 'descr')">
                            {{ note.descr }}
                    </span> 
                   <textarea 
                        class="textarea_hide"
                        ref="inputDescr"
                        v-model="note.descr"
                        @focus="noteBefore.oldDescr = note.descr"
                        @keyup.enter="editValue($event, note, 'descr')"
                        @keyup.esc="resetValue(note, 'descr')">
                            {{note.descr}}
                    </textarea>
                </p>
                <span>{{ note.date}}</span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        notes: {
            type: Array,
            required: true
        },
        grid: {
            type: Boolean,
            required: true
        },
        priority: {
            type: Array,
            required: true
        }
    },
    data(){
        return {
            noteBefore: {
                oldTitle: '',
                oldDescr: ''
            },
            editIndex: null
        }
    },
    computed: {
        getClassFull(){
            return (!this.grid) ? ' full' : '';
        }
    },
    methods: {
        // удаление заметки
        removeNote(index){
            console.log(`Note id - ${index} removed`);
            this.$emit('remove', index)
        },

        // изменить заметку
        editNote(index, name){
            this.editIndex = index;
            if(name == 'title'){
                this.setClass(this.$refs.inputTitle[index], 'input_show');
                this.$refs.inputTitle[index].focus();
            }
            if(name == 'descr'){
                this.setClass(this.$refs.inputDescr[index], 'input_show');
                this.$refs.inputDescr[index].focus();
            }
        },

        // изменение значения для заметки
        editValue(event, note, name){
            let newValue = event.target.value;
            let newClass = 'input_hide';
            if(name == 'title'){
                note.title = newValue;
                this.setClass(this.$refs.inputTitle[this.editIndex], newClass);
            }
            if(name == 'descr'){
                note.descr = newValue;
                this.setClass(this.$refs.inputDescr[this.editIndex], newClass);
            }
            note.date = new Date(Date.now()).toLocaleString();
        },

        // сброс изменений при редактировании
        resetValue(note, name){
            if(name == 'title'){
                note.title = this.noteBefore.oldTitle;
                this.setClass(this.$refs.inputTitle[this.editIndex], 'input_hide');
            }
            if(name == 'descr'){
                note.descr = this.noteBefore.oldDescr;
                this.setClass(this.$refs.inputDescr[this.editIndex], 'input_hide');
            }
        },

        // установка класса
        setClass(item, newClass){
            item.classList = newClass;
        }
    }
}
</script>

<style lang="scss">
.notes{
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    padding: 40px 0px;
}
.input{
    &_hide{
        display: none;
    }
    &_show{
        position: absolute;
        left: 0;
        top: 0;
        width: 350px;
        height: 110%;
    }
}
.textarea{
    &_hide{
        display: none;
    }
    &_show{
        position: absolute;
        top: 0;
        left: 0;
        height: 100%;
    }
}
.hide{
    position: absolute;
    opacity: 0.1;
    left: 0;
    top: 0;
    width: 400px;
    height: 100%;
}
.note{
    width: 48%;
    padding: 18px 20px;
    margin-bottom: 20px;
    background-color: #fff;
    transition: 0.25s;
    &-body{
        position: relative;
    }
    &__editor{
        position: relative;
    }
    &:hover{
        box-shadow: 0 30px 30px rgba(0,0,0,0.04);
        transform: translate(0, -6px);
        transition-delay: 0s !important;
    }
    &.full{
        width: 100%;
        text-align: center;
    }
    &_priority-medium, &_priority-hard{
        border: 3px solid orange
    }
    &_priority-hard{
        border-color: red
    }
}
.note-header{
    display: flex;
    align-items: center;
    justify-content: space-between;
    h1{
        font-size: 32px;
    }
    p{
        color: rgb(15, 15, 201);
        font-size: 22px;
    }
    svg{
        margin-right: 12px;
        cursor: pointer;
        color: #999;
        &:last-child{
            margin-right: 0;
        }
        &.active{
           color: rgb(15, 15, 201);
        }
    }
    &.full{
        justify-content: center;
        p{
            margin-right: 16px;
            &:last-child{
                margin-right: 0;
            }
        }
    }
}
.note-body{
    p{
        margin: 20px 0px;
    }
    span{
        font-size: 14px;
        color: #999;
        display: block;
        min-height: 17px;
    }
}
</style>