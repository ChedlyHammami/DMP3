<template>
    <div>

        <form @submit="converteV">
            <center>
                    <div class="content">
                    <b-form-input
                        id="url"
                        v-model="ulrUp"
                        :state="nameState"
                        aria-describedby="input-live-help input-live-feedback"
                        placeholder="Enter the video url"
                        trim
                        required
                    ></b-form-input>
                    <b-form-select
                        id="slt"
                        v-model="format"
                        :options="options"
                        class="mb-3"
                        value-field="item"
                        text-field="name"
                        disabled-field="notEnabled"
                    ></b-form-select>
                    </div>
                    <div class="b">
                <b-overlay :show="show" class="d-inline-block" width=50%>
                    <b-button thumbnail  fluid type="submit"  id="btn" class="btnn" v-show=conv>Convert</b-button>
                </b-overlay>
                <a v-bind:href=urlDownload v-show=download class="btnn" @click="afterDownload">Download</a>
                </div>
                
            </center>
        </form>
        <center><br>
            <div class="alert" v-show="errHandler">
                <span class="closebtn" @click="errHandler=!errHandler">&times;</span>  
                <strong>Warning!</strong> Enter a valid Url !!
            </div>
        </center>
    </div>
</template>
<script>

export default {
    name:"Search",
    data(){
        return{
            download:false,
            urlDownload:"",
            loading:false,
            conv:true,
            format:"mp3",
            ulrUp: '',
            checked:false,
            options: [
            { item: 'mp3', name: 'MP3' },
            { item: 'mp4', name: 'MP4' }
            ],
            show:false,
            errHandler:false
        }
    },
    computed: {
      nameState() {
        return this.ulrUp.length > 2 ? true : false
      }
    },
    methods:{
        async converteV(event){
            event.preventDefault();
            this.show = true;
            const urlUpload = {
                url:document.getElementById('url').value
            }
            console.log(urlUpload.url);
            const format = document.querySelector("select").options[document.querySelector("select").selectedIndex].value;
            console.log(format);
            const res = await fetch("https://dmp3-server.herokuapp.com/",{
                method:"POST",
                headers:{
                    'Content-Type':'application/json'
                },
                body:JSON.stringify({
                    url: urlUpload.url,
                    format
                })
            });
            
            let data = await res.json();
            if(data.url){
                this.show= false;
                this.conv= false;
                this.urlDownload=data.url;
                this.download=true;
            }
            else{
                this.show=false;
                this.errHandler=true;
            }
            
        },
        afterDownload(){
            this.download=false;
            this.conv= true;
        },
    
    }
    
}
</script>
<style scoped>
    .x{
        color:white;
        background-color: rgb(255, 22, 22);
        border:none;
        
    }
    .b{
        display:grid;
        grid-template-rows: auto auto;
        width:50%;
    }
    .alert {
        width: 50%;
        padding-top:10px;
        padding-bottom: 10px;
        background-color: #f44336;
        color: white;
        opacity: 1;
        transition: opacity 0.6s;
        margin-bottom: 20px;
    }
    .closebtn {
        margin-left: 15px;
        color: white;
        font-weight: bold;
        float: right;
        font-size: 22px;
        line-height: 20px;
        cursor: pointer;
        transition: 0.3s;
    }
  .btnn{
      transition:0.3s;
      text-decoration:none;
      color:white;
      background-color:black;
      padding-top:10px;
      padding-bottom:10px;
      border-radius: 5px;
      
  }
  .darkm{
      margin:auto;
      display:grid;
      grid-template-columns: auto 10% auto;
 
  }
    .btnn:hover{
        background-color:rgb(26, 26, 26);
        transition: 0.2s;
    }
    #btn{
        width:100%;
    }
    .alertt{
        display: inline-block;
        background-color:rgb(255, 22, 22);
        color:white;
        border-radius: 5px;
        
    }
    #url{
        width:100%;
        border-color:black;
    }
    #slt{
        width:100%;
        border-color:black;
    }
    .content{
        width:50%;
        display: grid;
        grid-template-columns: 80% 20%;
    }
    .linka{
        background-color:rgba(255, 0, 0, 0);
    }
  @media(max-width:800px){
    .content{
        display:block;
        margin-bottom:10px;
    }
    .btnn{
        padding-right:20%;
        padding-left:20%;
    }
  }
</style>