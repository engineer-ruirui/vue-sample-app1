<template>
  <div>
    <router-link to="/">Back</router-link>
    <br>
    <div class="card">
      <div class="card-body">
        <div class="form-group">
          <input v-model="siteName" placeholder="Site Name" class="form-control">
          <input v-model="siteURL" placeholder="Site Url" class="form-control">
          <button v-on:click="editSite()" class="btn btn-primary">EDIT</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      siteName: "",
      siteURL: ""
    }
  },
  created () {
    const id = this.$route.params.id;
    fetch(`http://localhost:3000/sites/${id}`)
    .then((res)=>res.json())
    .then((data)=> {
      this.siteName = data.name;
      this.siteURL = data.url;
    })
    .catch((err)=>console.log(err));
  },
  methods: {
    editSite () {
      if (this.siteName === "" || this.siteURL === "") {
        return;
      }
      const id = this.$route.params.id;
      const siteInfo = {
        name: this.siteName,
        url: this.siteURL
      }

      fetch(`http://localhost:3000/sites/${id}`, {
        method:'PUT',
        headers: {
          'Content-type':'application/json'
        },
        body: JSON.stringify(siteInfo)
      })
      .then(()=>{
        this.siteName = "";
        this.siteURL = "";
      })
      .catch((err)=>console.log(err));
    }
  }
}
</script>