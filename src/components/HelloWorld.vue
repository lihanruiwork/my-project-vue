<template>
  <div class="hello" 
    @mousemove="debounce(getUserAction,1000)"
  >
    <p >{{count}}</p>
    <p @click="suanfa1(100,2)">不用加减乘除做加法:100+2={{res1}}</p>
    <p>各种排序{{array}}</p>
    <p @click="maopao">冒泡排序{{maopaoarr}}</p>
    <p @click="xuanze">选择排序{{xuanzearr}}</p>
    <p @click="charu">插入排序{{charuarr}}</p>
    <p @click="guibing">合并排序{{guibingarr}}</p>
    <p @click="quick">快速排序{{quickarr}}</p>
    <p @click="flattern2">数组扁平化{{flatRes}}</p>
    <a href="/#/firstInterview">循环数组实现队列</a>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      count:0,
      res1: '',
      array: [5,5,4,4,3,3,2,1,2,4,7],
      flatArray: [1, ['2', [3, 4]]],
      maopaoarr:[],
      xuanzearr:[],
      charuarr:[],
      guibingarr:[],
      quickarr:[],
      flatRes:[]
    }
  },
  methods:{
    debounce(func,wait){
      var timeout;
      console.log("22222222");
      return function(){
        console.log("timeout===>", timeout);
        clearTimeout(timeout);
        timeout = setTimeout(func,wait);
      }
    },
    getUserAction(){
      this.count ++;
    },
    flattern1(){
      //缺点：数据类型自动string
      this.flatRes = this.flatArray.toString().split(',').map(i=>{
        return +i
      });
    },
    flattern2(){
      this.flatRes = this.flattern2Inner(this.flatArray);
    },
    flattern2Inner(arr){
      return arr.reduce((prev,next)=>{
        return prev.concat(Array.isArray(next) ? this.flattern2Inner(next):next)
      },[])
    },
    suanfa1(a,b){
      let sum = 0;
      let carry = 0;
      let sum1=a;
      let sum2=b;
      do{
        sum = sum1 ^ sum2;
        carry = (sum1&sum2)<<1;
        sum1 = sum;
        sum2 = carry;
      }
      while(carry!==0)
      this.res1 = sum;
    },
    swap(a, b,arr){
      console.log("swap===>", a, b);
      let tmp = arr[a];
      arr[a] = arr[b];
      arr[b] = tmp;
    },
    maopao(){
      let arr = [...this.array]
      for(let i=0;i<arr.length;i++){
        for(let j=0;j<arr.length-i-1;j++){
          if(arr[j]>arr[j+1]){
            this.swap(j,j+1,arr)
          }
        }
      }
      this.maopaoarr = arr;
    },
    xuanze(){
      let arr = [...this.array]
      for(let i=0; i<arr.length-1; i++){
        let min = i;
        for(let j=i+1; j<arr.length; j++){
          min = arr[min] > arr[j] ? j : min;
        }
        this.swap(min,i,arr)
      }
      this.xuanzearr = arr;
    },
    charu(){
      let arr = [...this.array]
      for(let i=0; i<arr.length-1; i++){
        let tmp = arr[i]
        for(var j=i-1; j>=0&&arr[j]>tmp; j--){
          arr[j+1]=arr[j]
        }
        arr[j+1] = tmp;
      }
      this.charuarr = arr;
    },
    guibing(){
      this.guibingarr = this.guibingInner([...this.array])
    },
    guibingInner(arr){
      if(arr.length<2){
        return arr;
      }
      let mid = arr.length/2;
      let left = arr.slice(0,mid);
      let right = arr.slice(mid,arr.length)
      return this.merge(this.guibingInner(left), this.guibingInner(right))
    },
    merge(left,right){
      let res = [];
      let i=0;
      let j=0;
      while(i<left.length&&j<right.length){
        if(left[i]<right[j]){
          res.push(left[i++]);
        }else{
          res.push(right[j++])
        }
      }
      return res.concat(left.slice(i)).concat(right.slice(j));
    },
    quick(){
      this.quickarr = this.quickInner([...this.array])
    },
    quickInner(arr){
      if(arr.length<2){
        return arr;
      }
      let mid = arr.length/2;
      let val = arr.splice(mid,1)
      let left=[];
      let right=[]
      for(let j=0;j<arr.length;j++){
        if(arr[j]<val){
          left.push(arr[j]);
        }else{
          right.push(arr[j])
        }
      } 
      return this.quickInner(left).concat(val, this.quickInner(right))
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
