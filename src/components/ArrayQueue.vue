<template>
  <div class="hello">
    <p>用循环数组实现一个队列</p>
    <p @click="enQueue">入队</p><p @click="deQueue">出队</p>
    <div>{{content}}</div>
  </div>
  
</template>

<script>
export default {
  name: 'array-queue',
  props: {
    max: {
      type: Number,
      default: 5
    }

  },
  data () {
    return {
      start:0, //队首指针
      end:0, //队尾指针
      arr: [],
      empty: true,
      content:[],
      element:1
    }
  },
  //[1,2,3,4,5]  0 0
  //6,2,3,4,5  1 1
  //6, ,3,4,5  2 1
  //6, , , ,    1
  // , , , ,   0 1
  //6,7,3,4,5  2 2
  // , , , ,   2 2
  mounted: function () {
    this.arr = [this.max]
    console.log(this.max);
  },
  methods:{
    enQueue(){
      if(this.isFull()){
        return ;
      }
      this.arr[this.end]=this.element++;
      if(this.end===this.max-1){
        this.end=0;
      }else{
        this.end++
      }
      this.empty=false
      this.showState()
    },
    deQueue(){
      if(this.isEmpty()){
        return ;
      }
      let res = this.arr[this.start]
      this.arr[this.start]='';
      if(this.start===this.max-1){
        this.start=0
      }else{
        this.start++
      }
      this.empty=true
      this.showState()
      return res
    },
    isFull(){
      return !this.empty && this.start===this.end
    },
    isEmpty(){
      return this.empty && this.start===this.end
    },
    showState(){
      this.content=[
        this.arr,
        this.isFull(),
        this.isEmpty()
      ]
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
