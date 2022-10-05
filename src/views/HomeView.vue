<template>
  <v-container>
        <v-row>
          <v-col
            v-for="book in books"
            :key="book.title"
            cols="12"
            md="3"
          >
            <v-card height="200"><h3>{{book.title}}</h3><h5>{{book.author}}</h5></v-card>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12">
            <div class="text-center">
                    <v-pagination
                    v-model="page"
                    total-visible="10"
                    :length="Math.ceil(totalBooks/perPage)"
                   ></v-pagination>
             </div>
          </v-col>
        </v-row>
      </v-container>
</template>

<script>


export default {
  name: 'HomeView',

  data(){
    return{
      books:[],
      page:1,
      totalBooks:0,
      perPage:20
    }
  },
  created(){
    console.log('created')
    this.getData();
  },
  methods:{
    getData(){
      let api="https://api.nytimes.com/svc/books/v3/lists/best-sellers/history.json"
      this.axios.get(api,{
        params:{
          'api-key':'mIWAvZ9z5WZ7upvcjBhWlBxSZn5rvv5k',
          'offset':this.perPage*(this.page-1)
        }
      }).then((response) => {
      console.log(response.data)
      this.books=response.data.results
      this.totalBooks=response.data.num_results
})
 
    }
  },

  watch:{
    page:function(){
      this.getData();
    }
  }
 
}
</script>
