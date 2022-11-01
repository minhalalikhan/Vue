<template>

    <h1>list of earthquakes</h1>
      <div class="controls">
    <span> min mag :</span><input type="number" max="10" min="1" v-model="magvar"  @input="checkforerr()" required />
      <span> limit :</span><input type="number" min="1" max="25" v-model="limit" @input="checkforerr()" required />
     <input type="date" v-model="startdate" />
   <button @click="updatequake" :class="{buttonwork: !haserror}"> search</button>
   <span class="err" v-if="haserror" >  error !! invalid input</span>
     <span class="load" v-if="isloading" >    ...loading </span>
    <!-- <span  > {{startdate}} </span> -->
       </div>
  <ul id="qlist">
      <li v-for="item in earthquakes" :key="item" :style="{backgroundColor:stylemag(item.properties.mag)}" >
         <div class="li-text"><span class="li-mag">{{formatmag(item.properties.mag)}} </span > <span class="li-loc"> {{item.properties.place}}</span></div>
         </li>
  </ul>

</template>

<script>
import axios from 'axios' 

export default {
  name: 'quakelist',
  props: {
   
  },
  
  data(){
     return{
     
     earthquakes:[],
     startdate: "2021-09-01",
magvar:1,
limit:5,
haserror:0,
isloading:0
     }
  },
  mounted(){
    this.isloading=1
this.getdate();
axios.get("https://earthquake.usgs.gov/fdsnws/event/1/query?format=geojson&starttime=2021-01-01&minmag="+this.magvar+"&limit="+this.limit)
 .then(res => {
   this.earthquakes=res.data.features
 this.isloading=0;   

 } ) 
 


  },
  methods :{
      formatmag(value){

       return value.toFixed(1)
      },

      stylemag(value){
        value=value.toFixed(1);
   value=255-(value*255/10)
    return  "rgb(255, "+value+", 97)";
      },
      updatequake(){
     
          if(this.haserror==1 )
        {//nothing for now
         }
          else{
          this.isloading=1
axios.get("https://earthquake.usgs.gov/fdsnws/event/1/query?format=geojson&starttime="+this.startdate+"&minmag="+this.magvar+"&limit="+this.limit)
 .then(res => {this.earthquakes=res.data.features
 this.isloading=0

 })

      }
      },
      checkforerr(){
          if(this.limit >25 || this.magvar >10 ||this.limit <1 || this.magvar <1  )
          this.haserror=1
          else
          this.haserror=0
      },
      getdate(){
        var today=new Date();
        var yy=today.getFullYear();
        var mm=today.getMonth()+1;
        var dd=today.getDate();
        if(dd<10) 
{
    dd='0'+dd;
} 

if(mm<10) 
{
    mm='0'+mm;
} 
       // return yy+'-'+mm+'-'+dd+'-';
       this.startdate=yy+'-'+mm+'-'+dd;
      
      }

  }
  
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->


<style scoped>



h1{
    font-size: 20px;
}
ul{
  margin: 0;
  padding: 0;
  width:100%;
}
span{
  font-weight: bold;
}
li{
   
    height:100px;
  font-weight: bold;
    margin:10px auto;
font-size: 30px;
width:80%;
box-shadow: 4px 4px 5px lightgray;

}
.li-text{
  background-color: white;
  width:80%;
  margin :0 auto;
  height:100%;
  display: flex;
}

.li-mag{
flex:0;
    margin: 0 auto;
    height: 80px;
    padding: 0 10px;
    font-size: 50px;
    display: inline-flex;
    align-items: center;
}

.li-loc{
flex:1;
display: block;
align-items: center;
font-size:20px;
}
.controls{
 margin:5px;
display: flex;

}

input{
 margin:0 20px;
 min-width:30px;
}
.list{
  background-color: lightcoral;
}
*{
  text-align: center;
  display: block;
}
.err{
  background-color: rgb(255, 180, 180);
  border:solid red 2px;
  padding: 4px;
}

.buttonwork{
  background-color:rgb(145, 0, 189);
  color: white;
  border: none;
  margin:4px;
  box-shadow:  5px 5px  15px darkgrey;
  padding: 10px;
  font-size: 15px;
  border-radius: 10px;
}

button{
  background-color:rgb(145, 0, 189);
  color: white;
  border: none;
  margin:4px;
  box-shadow:  5px 5px  15px darkgrey;
  padding: 10px;
  font-size: 15px;
  border-radius: 10px;
}
.buttonwork:active{
box-shadow: none;
}



@media screen and (max-width:400px)
{
.controls{
  display: flex;
  flex-direction: column;
}

 li{
    height:fit-content;
    width: 100%;
  }

.li-text{
  background-color: white;
  width:80%;
 
  height:100%;
  display: flex;
}

.li-mag{
flex:0;
    margin: 0 auto;
    height: 80px;
    padding: 0 10px;
    font-size: 50px;
    display: inline-flex;
    align-items: center;
}

.li-loc{
flex:1;
display: block;
align-items: center;
font-size:20px;
}
 
}
</style>
