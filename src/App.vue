<template>
<div class="wrapper">

  <div class="wrapper-content">
    <section>
      <div class="container">

        <!-- message -->
        <message v-if="message" :message="message" />

        <!-- new note -->
        <newNote :note="note" :priority="priorities" @addNote="addNote" />
 
        <div class="note-header">
          <!-- title -->
          <h1>{{ title }}</h1>
          
          <!-- search -->
          <search @search="search = $event" :value="search" placeholder="Find your note" />

          <!-- icons control -->
          <div class="icons">
            <svg :class="{ active: grid }" @click="grid = true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" ><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg>
            <svg :class="{ active: !grid }" @click="grid = false" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"></line><line x1="8" y1="12" x2="21" y2="12"></line><line x1="8" y1="18" x2="21" y2="18"></line><line x1="3" y1="6" x2="3" y2="6"></line><line x1="3" y1="12" x2="3" y2="12"></line><line x1="3" y1="18" x2="3" y2="18"></line></svg>
          </div>
        </div>
        
        <!-- note list -->
        <notes :notes="notesFilter" :grid="grid" :priority="priorities" @remove="removeNote" />
        
      </div>
    </section>
  </div>
</div>
</template>

<script>
import message from '@/components/Message.vue';
import notes from '@/components/Notes.vue';
import newNote from '@/components/NewNote.vue';
import search from '@/components/Search.vue';

export default {
  components: {
    message, notes, newNote, search
  },
  data(){
    return {
      title: 'Notes App',
      search: '',
      message: null,
      grid: true,
      note: {
          title: '',
          descr: '',
          priority: 'easy'
      },
      notes: [
        {
            id: 0,
            title: 'First Note',
            descr: 'Description for first note',
            date: new Date(Date.now()).toLocaleString(),
            priority: 'easy'
        },
        {
            id: 1,
            title: 'Second Note',
            descr: 'Description for second note',
            date: new Date(Date.now()).toLocaleString(),
            priority: 'medium'
        },
        {
            id: 2,
            title: 'Third Note',
            descr: 'Description for third note',
            date: new Date(Date.now()).toLocaleString(),
            priority: 'hard'
        }
      ],
      priorities: [
        {alias: 'easy', title: 'Standart'},
        {alias: 'medium', title: 'Important'},
        {alias: 'hard', title: 'Very Important'},
      ]
    }
  },
  computed: {
    notesFilter(){
      let allNotes = this.notes,
          search = this.search;
      if(!search) return allNotes;
      search = search.trim().toLowerCase();
      allNotes = allNotes.filter((el) => {
        if(el.title.toLowerCase().indexOf(search) != -1){
          return el
        }
      });
      // Error
      return allNotes
    }
  },
  methods: {
    addNote(){
      let {title, descr, priority} = this.note;
      if(title === '') {
          this.message = 'title cannot be empty';
          return false
      }
      this.notes.push({
          id: (Math.random()).toFixed(10),
          title,
          descr,
          date: new Date(Date.now()).toLocaleString(),
          priority
      });
      this.note.title = '';
      this.note.descr = '';
      this.note.priority = 'easy';
      this.message = null;
    },
    
    removeNote(id){
      this.notes = this.notes.filter(el => el.id != id)
    }
  }
}
</script>

