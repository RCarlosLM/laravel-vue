<template>
  <div>
    <h2 class="text-center">
      Captura tus ideas
    </h2>
    <div class="card card-body bg-light">
      <h4>
        ¿En que estas pensando?
      </h4>
      <form v-on:submit.prevent="createIdea">
        <div class="input-group">
          <input v-model="newIdea" type="text" class="form-control input-sm" maxlength="256">
          <span class="input-group-btn">
            <button type="submit" class="btn btn-primary btn-md">
              Agregar
            </button>
          </span>
        </div>
      </form>
      <hr>
      <ul class="list-unstyled">
        <li v-for="idea in ideas">
          <p>
            <small class="text-muted">
              <em>
                {{ since(idea.created_at) }}
              </em>
            </small>
            {{ idea.description }}
          </p>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'
  import toastr from 'toastr'
  import moment from 'moment'

  moment.locale('es');//lang

  export default {
    data () {
      return {
        ideas: [],
        newIdea: ''
      }
    },
    created: function(){
      this.getIdeas();
    },
    methods: {
      since: function(d){
        return moment(d).fromNow();
      },
      getIdeas: function(){
        var urlIdeas = 'mis-ideas';
        axios.get(urlIdeas).then(response => {
          this.ideas = response.data
        });
      },
      createIdea: function(){
        var urlNewIdea = 'guardar-idea';
        axios.post(urlNewIdea,{
          description: this.newIdea
        }).then( response => {
          this.getIdeas();
          this.newIdea = '';
          toastr.success('Nueva Idea Registrada');
        }).catch(error => {
          toastr.error('Error');
        });
      }
    }
  }
</script>
