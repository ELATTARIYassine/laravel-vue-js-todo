<template>
  <div class="w-30">
    <form @submit.prevent="saveDate">
      <div class="input-group mb-3 w-100">
        <input
          type="text"
          class="form-control form-control-lg"
          placeholder="Recipient's username"
          aria-label="Recipient's username"
          aria-describedby="basic-addon2"
          v-model="form.title"
          :class="{'is-invalid': form.errors.has('title')}"
          @keydown="form.errors.clear('title')"
        />
        <div class="input-group-append">
          <button class="btn btn-success" type="submit">Add Todo to your list</button>
        </div>
      </div>
      <span class="text-danger pt-3 pb-3" 
            v-if="form.errors.has('title')" 
            v-text="form.errors.get('title')"
            style="font-size:20px;"></span>
    </form>
    <div class="w-25">
        <div v-for="todo in todos" :key="todo.id" class="w-100">
            {{ todo.title }}
        </div>
    </div>
  </div>
</template>

<script>

export default {
    data() {
        return {
            todos: [],
            form: new Form({
                title: ''
            })
        };
    },
    methods: {
        getTodos(){
            axios.get('/api/todo').then((res) => {
                this.todos = res.data;
            }).catch((error) => {
                console.log(error);
            });
        },
        saveDate(){
            let data = new FormData();
            data.append('title', this.form.title);
            axios.post('/api/todo', data).then((res) => {
                this.form.reset();
                this.getTodos();
            }).catch((error) => {
                this.form.errors.record(error.response.data.errors);
            });
            
        }
    },
    mounted() {
        console.log("Component mounted.");
        this.getTodos();
    },
    };
</script>
