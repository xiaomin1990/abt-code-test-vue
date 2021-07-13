<template>
  <div id="app">
    <div class="header">
      <div class="searchBox"> 
        <input type="text" placeholder="input your blockHash" v-model="blockhash" >
        <label class="error">{{errormsg}}</label>
      </div>
      <label class="btn" v-on:click="search">查询</label>
    </div>
    <div class="title">Block {{responsedata.block_index}}</div>
    <div class="list">
      <div class="item">
        <label>Hash</label> <label>{{responsedata.hash}}</label>
      </div>
       <div class="item">
        <label>Confirmations</label> <label>1</label>
      </div>
       <div class="item">
        <label>Timestamp</label> <label>{{responsedata.time}}</label>
      </div>
        <div class="item">
        <label>Height</label> <label>{{responsedata.height}}</label>
      </div>
       <div class="item">
        <label>Miner</label> <label>Poolin</label>
      </div>
       <div class="item">
        <label>Number of Transactions</label> <label>{{responsedata.n_tx}}</label>
      </div>
       <div class="item">
        <label>Difficulty</label> <label></label>
      </div>
        <div class="item">
        <label>Merkle root</label> <label>{{responsedata.mrkl_root}}</label>
      </div>
        <div class="item">
        <label>Version</label> <label>0x20000000</label>
      </div>
       <div class="item">
        <label>Bits</label> <label>{{responsedata.bits}}</label>
      </div>
        <div class="item">
        <label>Weight</label> <label>{{responsedata.weight}}</label>
      </div>
       <div class="item">
        <label>Size</label> <label>{{responsedata.size}} bytes</label>
      </div>
       <div class="item">
        <label>Nonce</label> <label>{{responsedata.nonce}} </label>
      </div>
         <div class="item">
        <label>Transaction Volume</label> <label></label>
      </div>
         <div class="item">
        <label>Block Reward</label> <label> </label>
      </div>
       <div class="item">
        <label>Fee Reward</label> <label>{{responsedata.fee}} </label>
      </div>
    </div>
    <div class="title">Block Transactions</div>
    <div class="list ">
      <div class="item" v-for="item in tx"  :key="item.hash">
        <label>Hash</label> <label>{{item.hash}}</label><label>{{item.time}}</label>
        <label>Fee</label> <label>{{item.fee}}</label>
      </div>
    </div>
    <div class="page">
      <div v-on:click="page(0)">1</div> <div v-on:click="page(1)">2</div> <div v-on:click="page(2)">3</div> <div v-on:click="page(3)">4</div> <div v-on:click="page(4)">5</div><div>...</div>
    </div>
  </div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
const axios = require('axios');

export default {
  name: 'App',
  components: {
    //HelloWorld
  },
   data:function() {
    return {
      errormsg: '',
      blockhash:"",
      responsedata:{},
      tx:[]
      }
  },
   mounted () {
     this._search("00000000000000000007878ec04bb2b2e12317804810f4c26033585b3f81ffaa")
   },
  methods:{
    search(){
      this.errormsg="";
      if(this.blockhash){
         this._search(this.blockhash)
      }else{
          this.errormsg="请输入blockHash";
      }
    },
    page(index){
      console.log('index:',index)
      this.tx = this.responsedata.tx.slice(index*5, (index+1)*5)
      },
    _search(blockhash,index=0){
      axios
        .get(`http://127.0.0.1:7001/home/show/${blockhash}`)
        .then(response => {
             if(response && response.status==200 && response.data && !response.data.error){
                console.log(response.data)
                this.responsedata = response.data
                this.tx = response.data.tx && response.data.tx.slice(index*5, (index+1)*5)
              }else{
                  this.errormsg=`请求出错了，原因：${response.data.error}`
                }
       })
    }
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #000;
  margin: 20px ;
  display: flex;
  flex-direction: column;
}
.header{
    display: flex;
    justify-content: flex-end;
    align-items:flex-start;
    
  }
  .header .btn{
    width: 100px;
    min-width: 50px;
    height: 32px;
    line-height: 32px;
    text-align: center;
    padding: 5px 10px;
    border: 1px solid rgb(223, 227, 235);
    border-radius: 5px;
    margin-left: 30px;
    cursor: pointer;
  }
  .header .searchBox{
    display: flex;
    flex-direction: column;
  }
  .header .searchBox input{
    min-width: 400px;
    height: 32px;
    line-height: 32px;
    padding: 5px 10px;
    border: 1px solid rgb(223, 227, 235);
    border-radius: 5px;
  }
  .header .searchBox label{
    font-size: 10px;
    color: red;
    margin: 2px 10px;
  }

  .errormsg{
    width: 98%;
    text-align: center;
    color: red;
    font-size: 12px;
  }
  .title{
    font-size: 16px;
    font-weight: bold;
    margin: 10px 0;
  }
 .list{
   max-width: 80%;
   display: flex;
   flex-direction: column;
  
  }
.list .item{
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid rgb(223, 227, 235);
  margin:10px 20px;
  padding: 5px 10px;
  }
  .page{
    display: flex;
    margin: 10px 0;
  }
  .page div{
    width: 30px;
    text-align: center;
    padding: 2px 5px;
    margin: 0 1px;
    border: 1px solid rgb(223, 227, 235);
    border-radius: 5px;
    cursor: pointer;
  }
</style>
