<template>
    <p class="description">这是一个打字游戏。</p>
    <div class="gamebox" v-if="gameIsStart">
        <div v-for="(item,id) in letters" :key="id">
            <div class="letterbox" :style="{left : item.posleft , top:item.posup}">
                {{item.alpha}}
            </div>
        </div>
        <div class="score">您的分数：{{score}}</div>
        <p class="end" v-if="gameEnd">游戏结束！</p>
        <button @click="mytest" v-if="debugMode"></button>
    </div>
    <div class="settings" v-else>
        <h1>设置界面</h1>
        
        <br>
        字母下落速度：范围 1 ~ 10之间：
        <br>
        <input type="text" placeholder="请输入字母下落速度" v-model="speed">
        <br>
        每次出现字母个数：范围 1 ~ 10之间：
        <br>
        <input type="text" placeholder="请输入字母出现个数" v-model="addNum">
        <br>
        每次下落时间间隔：（单位：ms）
        <br>
        <input type="text" placeholder="请输入字母出现个数" v-model="addInterval">
        <br>
        <button @click="gameStart">开始游戏</button>
    </div>
</template>
<script>



export default{
    data(){
        return{
            letters:[{id:1,alpha:'A',posleft:'50%',posup:'20%',hit:false}],
            score:0,
            nextid:2,
            gameIsStart:false,
            gameEnd:false,
            debugMode:false,
            speed:2,
            addNum:2,
            addInterval:2000,
        }
    },
    created(){
    },
    methods: {
        getrandom(min,max){
            return Math.floor(Math.random()*(max-min+1)+min)
        },
        gameStart(){
            this.gameIsStart=true
            let timer=setInterval(() => {
            this.letters.filter(item=>{
                    item.posup=parseFloat(item.posup)+(item.hit?(-1*this.speed):(0.1*this.speed))+'%'
                    if (parseFloat(item.posup)>100 && !item.hit && !this.debugMode){
                        clearInterval(timer)
                        clearInterval(addletter)
                        this.gameEnd=true
                    }
                })
                
            }, 10);
            let addletter=setInterval(()=>{
                for (let i=0;i<this.addNum;++i){
                        this.letters.push({
                        id:this.nextid,
                        alpha: String.fromCharCode('A'.charCodeAt()+this.getrandom(0,25)),
                        posleft: this.getrandom(0,90)+'%',
                        posup: '0%',
                        hit:false,
                    })
                    this.nextid++
                }
                
            },this.addInterval)
            document.onkeydown=(e)=>{
                //console.log(e.key.toUpperCase())
                let res=this.letters.some((item)=>{
                    if (e.key.toUpperCase()===item.alpha && !item.hit){
                        //console.log('hit!')
                        item.hit=true
                        this.score++
                        setTimeout(()=>{
                            this.letters=this.letters.filter(i=>i.id!=item.id)
                        },1500)
                        return true;
                    }
                })
                if (!res){
                    this.score-=3
                }
            }
        },
        mytest(){
            this.letters.push({id: this.nextid,alpha:'Y',posleft:Math.random()*90+'%',posup:'0%',hit:false})
            this.nextid+=1
        },
            
    },
}
</script>
<style>
.gamebox{
    width: 80%;
    height: 800px;
    background-color: black;
    margin:30px auto;
    position: relative;
    overflow:hidden;
}
.gamebox .letterbox{
    width: 50px;
    height: 50px;
    border-radius: 25px;
    line-height: 50px;
    font-size:40px;
    background-color: transparent;
    color:Red;
    position: absolute;
    top:0px;
}
.description{
    
    font-family: '宋体';
    color:red;
    width: 80%;
    margin:0px auto;
    background-color: cyan;
    height:50px;
    font-size:40px;
}
.gamebox .score{
    background-color: transparent;
    color:yellow;
    font-family: '宋体';
    font-size:40px;
    position:absolute;
    left:0;
    bottom:0;
}
.gamebox .end{
    width: 100%;
    height: 100%;
    background-color: rgba(0,200,0,0.2);
    position:absolute;
    top:0px;
    left:0px;
    font-size: 50px;
    font-family: '宋体';
    color:red;
    margin:0px;
    line-height: 800px;
}
.settings{
    width: 80%;
    height: 800px;
    background-color: black;
    margin:0px auto;
    position: relative;
    text-align:center;
    font-size: 20px;
    font-family: '宋体';
    color:white;
}
.settings h1{
    font-size: 50px;
    font-family: '宋体';
    font-weight:700;
    color:red;
}
.settings input{
    width: 50%;
    height: 30px;
    margin:20px auto;
    font-size: 20px;
}
.settings button{
    width:50%;
    border-radius: 3px;
    height:50px;
    background-color: orange;
    color:white;
    margin: 100px auto;
    cursor:pointer;
    font-size:30px;
    font-family: '宋体';
    font-weight:700;
}
.settings button:hover{
    background-color: green;
    color:blue;
    transition: all .5s;
    scale:1.2;
}
</style>