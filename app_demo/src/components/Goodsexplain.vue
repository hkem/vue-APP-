<template>
  <div class="goodsexplain">
    <div class="goodsexplain-centre">
      <div class="goodsexplain-top-block">
        <van-nav-bar
          title="说明"
          left-text=""
          right-text=""
          left-arrow
          @click-left="onClickLeft"
        >
        </van-nav-bar>
      </div>
      <div class="goodsexplain-block">
        <div class="allcomm-error-block" v-if="isError">
          <van-empty image="error" description="网络错误,请刷新" />
        </div>
        <!-- 请求中 -->
        <div class="artdateils-loading-block" v-if="request_Loading">
          <Loading></Loading>
        </div>
        <div class="goodsexplain-nr-block">
          <div class="goodsexplain-nr" v-html="centre">

          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Errora from './Errora.vue';
  import Loading from './Loading.vue';
  export default {
    name: 'Goodsexplain',
    data () {
      return {
        msg: 'Welcome to Your Vue.js App',
        request_Loading:true,
        isLoading: false,
        isCentre:false,
        isError:false,
        centre:""
      }
    },
    methods:{
      onClickLeft:function(event){
        this.$router.go(-1);
      },
    },
    components: {
        Errora,
        Loading
    },
    created() {
      this.$axios
           .post(this.$myStore.url+"/api/index/getsetup",this.$qs.stringify({}))
           .then(response=>{
              //成功
              var data = response.data;
              console.log("成功");
              if(response.status != 200){
                this.$toast("获取失败");
              }else{
                if(data.code == 1){
                  var resdata = data.data;
                  this.isCentre = true;
                  this.request_Loading = false;
                  console.log(resdata);
                  this.centre = resdata.list.goodsexplain;
                }else{
                  this.isError = true;
                }
              }
           })
           .catch(error=>{
              //失败
              if(error.response){
                this.request_Loading = false;
                this.isCentre = false;
                this.isError = true;
              }


       });
    }
  }
</script>

<style>
.goodsexplain{
    width: 100%;
    height: 100%;
    position: relative;

  }
  .goodsexplain-center{
    float: left;
    width: 100%;
    background-color: #f5f5f5;
  }
  .goodsexplain-top-block{
    float: left;
    width: 100%;
    border-bottom: 2px solid #e0e0e0;
    position: fixed;
    top: 0px;
    background-color: #FFFFFF;
    z-index: 1000;
  }
  .goodsexplain-block{
    width: 100%;
    float: left;
    margin-top: 95px;
  }
  .goodsexplain-nr-block{
    width: 100%;
    float: left;
  }
  .goodsexplain-nr{
    width: 680px;
    margin: auto;
  }
  .goodsexplain-nr img{
    width: 680px;
  }
</style>
