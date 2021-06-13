<template>
    <v-row style="height:100%;width:100%" class="pa-0 ma-0">
      <v-col  sm="1" xs="1" md="6" lg="6" xl="6" cols="12" style="background-color:black">
        <editor  ref='myEditor' v-model="content" @init="editorInit" lang="html" theme="twilight" width="100%" height="100%"></editor>
      </v-col>
      <v-col  sm="1" xs="1" md="6" lg="6" xl="6" cols="12" class="pa-0 ma-0">
        <iframe allow referrerpolicy="same-origin" sandbox="allow-same-origin" :srcdoc="htmlContent" width="100%" height="100%"></iframe>
      </v-col>
    </v-row>
</template>

<script>
  const AceEditor = require('vue2-ace-editor')

  export default {
    name: 'Home',
    components:{
      editor: AceEditor,
    },
    data(){
      return{
        htmlContent : '',
        content: ''
      }
    },
    methods:{
      editorInit: function () {
            this.htmlContent = this.content;
            require('brace/ext/language_tools') //language extension prerequsite...
            require('brace/mode/html')                
            require('brace/mode/javascript')    //language
            require('brace/mode/less')
            require('brace/theme/twilight')
            require('brace/snippets/javascript') //snippet
      },
      setHTMLContent(){
        let annotations = this.$refs.myEditor.editor.getSession().getAnnotations();
        if(annotations.find((a) => a.type === "error")){
          this.showError();
        }else{
          this.htmlContent = this.content;
        }
      },
      download(){
        var textFileAsBlob = new Blob([this.content], {type:'text/plain'}); 
        var downloadLink = document.createElement("a");
        downloadLink.download = "download.txt";
        downloadLink.innerHTML = "Download File";
        if (window.webkitURL != null)
        {
          // Chrome allows the link to be clicked
          // without actually adding it to the DOM.
          downloadLink.href = window.webkitURL.createObjectURL(textFileAsBlob);
        }
        else
        {
          // Firefox requires the link to be added to the DOM
          // before it can be clicked.
          downloadLink.href = window.URL.createObjectURL(textFileAsBlob);
          downloadLink.style.display = "none";
          document.body.appendChild(downloadLink);
        }
      
        downloadLink.click();
      },
      showError(){
        let errors = this.$refs.myEditor.editor.getSession().getAnnotations();
        let errorHTML = '<h2>ERROR OCCURED. Unable to Load html</h2> <hr>';
        errors.forEach((e)=>{
          if(e.type === "error"){
            errorHTML += "<h4 style='color:red'>Row:"+ e.row + " Column:"+ e.column + " Error: "+ e.text +"</h4>"
          }
          if(e.type === "info"){
            errorHTML += "<h4 style='color:orange'>Row:"+ e.row + " Column:"+ e.column + " Info: "+ e.text +"</h4>"
          }
        });
        this.htmlContent = errorHTML
      }
    }
  }
</script>
