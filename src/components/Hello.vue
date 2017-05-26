<template>
  <div id="main">
    <input type="text" name="gh-username" placeholder="Search for a GitHub username..." v-model="username" v-on:keydown.13="search">
    <span class="loading" v-if="loading">Searching GitHub for "{{ username }}"...</span>
    <div class="results" v-if="results && !loading">
        <img v-if="results.avatar_url" :src="results.avatar_url" :alt="'GitHub Avatar for ' + username ">
        <h4 v-if="results.name">{{ results.name }}</h4>
        <i v-if="results.bio">{{ results.bio }}</i>
        <p v-if="results.location">{{ results.location }}</p>
    </div>
    <div class="results" v-if="error && !loading">
        Sorry, can't find a user called "<i>{{ username }}</i>"!
    </div>
  </div>
</template>

<script>
    export default {
        data () {
            return {
                username: '',
                results: '',
                error: '',
                loading: false,
            }
        },
        watch: {
            username: function (val) {
                this.error = '';
            }
        },
        methods:  {
            search: function() {

                if( this.username ){

                    this.loading = true;

                    console.log("Searching for... " + this.username );

                    axios.get('https://api.github.com/users/' + this.username)
                        .then(response => {

                            console.log( response.data );

                            this.results = response.data;
                            this.error = '';

                            this.loading = false;

                        }).catch(error => {

                            this.results = '';
                            this.error = error;

                            this.loading = false;

                        });

                } else {
                    this.results = '';
                    this.error = '';

                    this.loading = false;
                }

            }
        }
    }
</script>

<style scoped>
div {
   font-family: "HelveticaNeue-Light", "Helvetica Neue Light", "Helvetica Neue", Helvetica, Arial, "Lucida Grande", sans-serif;
   font-weight: 300;
   font-size: 16px; line-height: 1.4;
}
input {
    display: block;
    width: 350px;
    max-width: 74%;
    padding: 15px 25px;
    margin: 45px auto;
}
.results {
    display: block;
    width: 350px;
    max-width: 74%;
    padding: 25px;
    margin: 15px auto;
    background: #EEEEEE;
    border: 1px solid #D2D7D3;
    text-align: center;
}
.results img {
    display: block;
    width: 100px;
    height: auto;
    margin: 0 auto 15px;
}
.loading {
    display: block;
    text-align: center;
    color: #6C7A89;
}
</style>
