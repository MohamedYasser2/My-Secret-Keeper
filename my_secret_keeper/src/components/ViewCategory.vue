<template>
    <div>
  <v-dialog v-model="view" width="500">
        <template v-slot:activator="{ on, attrs }">
            <v-card flat v-for="(category,index) in categories" 
            :key="index" 
            class="mx-auto" 
            v-bind="attrs"
            v-on="on"
            @click="setCurrent(index)">
                <v-card-text>
                <v-app-bar
                    flat
                    color="rgba(0, 0, 0, 0)">
                    <v-toolbar-title class="text-h6 black--text pl-0">
                    {{category.name}}
                    </v-toolbar-title>
                    <v-spacer></v-spacer>
                    <v-menu
              top
              :offset-x="offset"
            >
              <template v-slot:activator="{ on, attrs }">
                <v-btn
              color="black"
              icon
               v-bind="attrs"
               v-on="on"
              @click="setCurrent(index)"
            >
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
              </template>
              <v-list>
                  <v-list-item>
                    <v-btn
                        color="primary"
                        text
                        @click="deleteCategory"
                    >
                        Delete
                    </v-btn>
                  </v-list-item>
              </v-list>
            </v-menu>
                </v-app-bar>
                </v-card-text>
                <v-divider></v-divider>
            </v-card>
        </template>


        <v-card>
            <v-card-title class="text-h5 grey lighten-2">
                {{curCategory.name}}
            </v-card-title>
        </v-card>
    </v-dialog>
    </div>
</template>

<script>
import CategoryRepository from "../data/category/repository/category_repository.vue"
import { getAuth } from 'firebase/auth'

export default {
  data() {
    return {
        ind:0,
        view: false,
      categories:[],
      curCategory:null
    };
  },
  methods: {
      setCurrent(i){
        this.curCategory=this.categories[i];
      },
      async deleteCategory(){
        var response = await CategoryRepository.methods.deleteCategory(this.curCategory.id);
        if(response === "Category is deleted successfully.") {
          this.view = false;
          this.categories = await CategoryRepository.methods.retrieveCategories();
          this.curCategory = this.categories[0];
        } else {
          alert(response);
        }
      },
  },
  async mounted() {
    getAuth().onAuthStateChanged(user => {
        if (user) {
          CategoryRepository.methods.retrieveCategories().then((data) => {
            this.categories = data; 
            this.curCategory = this.categories[0];
          });
        }
    });
  },
  created: async function() {
    this.categories=await CategoryRepository.methods.retrieveCategories();
    this.curCategory=this.categories[0];
  },
};
</script>