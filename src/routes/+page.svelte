<script>
    import {delay,num} from "./stores.js"
    import {flip} from "svelte/animate"
    let ValArr = []
    let StateArr = []
    let genid;
    let inde=0
    let nexinde=1
    $: reactive($num,$delay)
    function * bubbleSort(ValArr){
        for(let i=0;i < ValArr.length;i++){
            inde=0
            nexinde=1
            StateArr[inde]=true
            StateArr[nexinde]=true
            for(let j=0; j<ValArr.length-i-1;j++){
                if(ValArr[inde]>ValArr[nexinde]){
                    yield [ValArr[inde],ValArr[nexinde]]=[ValArr[nexinde],ValArr[inde]];
                }
                StateArr[inde]=false
                StateArr[nexinde]=false
                inde+=1
                nexinde+=1
                StateArr[inde]=true
                StateArr[nexinde]=true
            }
        }
        return;
    }
    const flippinsortin = (ValArr,delay)=>{
                let gen = bubbleSort(ValArr)
                genid = setInterval(()=>{
                    const {done}=gen.next();
                    if(done){
                        clearInterval(genid)
                    }
                },delay)
    }
    const reactive = (num,delay)=>{
        clearInterval(genid)
        let i = 0;
        ValArr=[]
        while (i<num) {
            ValArr.push(Math.random()*100)
            i+=1
        }
        StateArr = Array(num).fill(false)
        flippinsortin(ValArr,delay)
    }
</script>
<main>
    <div class="h11 mt-1 mb-8 mx-auto w-fit text-orange-600 font-mono font-extrabold">Bubble sort visualiser</div>
    <div class="barplotarea  mx-12 h-96 mb-16 border-t-4 border-black bg-gray-200">
        <ul class="w-full h-full grid grid-flow-col lg:gap-4 sm:gap-0">
            {#each ValArr as i,k (i) }
                <li class:concern={StateArr[k]} animate:flip="{{duration:$delay}}" class="border-x-4 border-b-4 border-black bg-lime-400" style="height: {i}%;" ></li>
            {/each}
        </ul>
    </div>    
</main>
<style>
    .concern{
        background: red !important;
    }
</style>
