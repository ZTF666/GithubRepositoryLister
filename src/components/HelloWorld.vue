<template>
  <div class="container">
  
    <div class="row">
      <h3 class="mx-auto">Github Repository Lister</h3>
      <h2 class="mx-auto">helps you download information about your personal list of repositories</h2>
      <p class="mx-auto">Brought to you by ZTF</p>
    </div>
    <div class="row">
      <div class="col">
        <input @keyup.enter="searchRepos" v-model="form.reponame" type="text" class="form-control" placeholder="Search for repositories..."/>
        <ul class="list-group">
          <li v-for="(repo, index) in searchResults" :key="index" class="list-group-item">
            {{repo.full_name}} ({{repo.stargazers_count}})
            <button v-if="reposToCompare.map(oneRepoToCompare => oneRepoToCompare.id).includes(repo.id)" @click="Retract(repo)" class="btn btn-sm btn-danger">Retract</button>
            <button v-else @click="addToComparisonDock(repo)" class="btn btn-sm btn-success">Add to comparision list</button>
            

            </li>
        
        </ul>
      </div>
      
      <div class="col">
        <table class="table">
          <thead>
            <tr>
              <th>Name</th>
              <th>Language</th>
              <th>Stars</th>
              <th>Watchers</th>
              <th>Forks</th>
              
              
            </tr>
          </thead>
          <tbody>
            <tr v-for="repo in reposToCompare" :key="repo.id" >
            <td>{{repo.full_name}}</td>
            <td>{{repo.language}}</td>
            <td>{{repo.stargazers_count}}</td>
            <td>{{repo.watchers_count}}</td>
            <td>{{repo.forks_count}}</td>
            
            
            </tr>
            
            <tr><td colspan="5"><button  class="btn btn-sm btn-info">Export</button></td></tr>
            
          </tbody>

        </table>
      </div>
    </div>

  </div>


</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
    return{
      form:{
        reponame:""
      },
      searchResults:[],
      reposToCompare:[]
    }
    
  },
  methods:{
    searchRepos(){
      let apiUrl="https://api.github.com/search/repositories?q=";
      apiUrl=apiUrl+this.form.reponame;
      
      fetch(apiUrl)
      .then(response=>response.json())
      .then(data=>{
        this.searchResults=data.items;
      })
      
    },
    addToComparisonDock(repo){
      this.reposToCompare.push(repo);
    },
    Retract(repo){
      const idOfRep=this.reposToCompare.indexOf(repo);
      this.reposToCompare.splice(idOfRep,1);
    }

  }
};
</script>



