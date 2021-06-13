<template>
  <v-app>
    <v-app-bar
      app
      color="danger"
      :dark="theme"
    >
      <div class="d-flex align-center">
        <v-btn small color="white" class="black--text dense" @click="fillHTMLContent">RUN</v-btn>
        <v-icon @click="changeTheme" class="ml-3" large >mdi-theme-light-dark</v-icon>
        <v-icon @click="changeOrientation" class="ml-3" large >mdi-phone-rotate-landscape</v-icon>
        <v-icon @click="saveFile" class="ml-3" large >mdi-content-save</v-icon>
        
      </div>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main>
      <HomeView ref="childComponent"/>
    </v-main>
  </v-app>
</template>

<script>
const HomeView = () => import("./views/Home.vue")
export default {
  name: 'App',
  data(){
    return{
      theme: true
    }
  },
  components:{
    HomeView
  },
  methods:{
    fillHTMLContent: function() {
        this.$refs.childComponent.setHTMLContent();
    },
    saveFile: function(){
       this.$refs.childComponent.download();
    },
    changeTheme: function(){
      this.theme= !this.theme;
      this.$refs.childComponent.changeTheme();
    },
    changeOrientation: function(){
      this.$refs.childComponent.changePosition();
    }
  }
};
</script>
