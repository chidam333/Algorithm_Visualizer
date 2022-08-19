<script>
    import { onMount } from "svelte";
    import {flip} from "svelte/animate"
    import "../app.css";
    import {browser} from "$app/env"
    let done=true
    let delay=1000
    let ValArr = []
    let StateArr = []
    let inde=0
    let nexinde=1
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
        if(done===false || true){
            setTimeout(
            ()=>{
                const gen = bubbleSort(ValArr)
                const genid =setInterval(()=>{
                    const {done}=gen.next();
                    console.log(done)
                    if(done){
                        clearInterval(genid)
                    }
                },1)
            }    
            ,1000)
        }

    }
    const reactive= (num,delay)=>{
        let i = 0;
        ValArr=[]
        while (i<num) {
            ValArr.push(Math.random()*100)
            i+=1
        }
        StateArr = Array(num).fill(false)
        flippinsortin(ValArr,delay)
    }
    let num = 7;
    reactive(num,delay)

</script>
<nav class="nav w-full lg:h-20 bg-orange-700 grid lg:grid-flow-col lg:gap-12 px-12">
    <div class="range h-full grid items-center">
        <label for="r" class="font-extrabold text-lime-200 grid justify-center">No. of values ( 5 - 35 )</label>
        <input type="range" id="r" min="5" max="35" class="w-full" bind:value={num} on:input={reactive(num,delay)}>
    </div>
    <div class="AlgoVishu flex lg:flex-col 1 -skew-x-12 mx-auto text-lime-200 font-extrabold tracking-widest"><span class="lg:-ml-8">Sorting &nbsp;</span><span>Algorithm &nbsp;</span><span class="lg:ml-8">Visualiser</span></div>
    <input type="number" placeholder="Quantity of delay (ms)" class= "hmm h-10  my-auto rounded-3xl border-blue-900 border-2 pl-4" bind:value={delay} on:input={reactive(num,delay)}>
</nav>
<div class="choose h-12 w-screen grid grid-flow-col mt-8 lg:px-36"><button class="btn mr-2 bg-zinc-900 h-full border-x-4 border-cyan-100 -skew-x-12 text-cyan-50">Bubble sort</button>
<button class="btn mr-2 bg-zinc-900 h-full border-x-4 border-cyan-100 -skew-x-12 text-cyan-50">Insertion sort</button>
<button class="btn mr-2 bg-zinc-900 h-full border-x-4 border-cyan-100 -skew-x-12 text-cyan-50">Merge sort</button>
<button class="btn mr-2 bg-zinc-900 h-full border-x-4 border-cyan-100 -skew-x-12 text-cyan-50">Quick Sort</button>
</div>
<slot />
<div class="barplotarea  mx-12 h-96 my-12 border-t-4 border-black bg-gray-200">
    <ul class="w-full h-full grid grid-flow-col lg:gap-4 sm:gap-0">
        {#each ValArr as i,k (i) }
            <li class:concern={StateArr[k]} class="border-x-4 border-b-4 border-black bg-lime-400" style="height: {i}%;" ></li>
        {/each}
    </ul>
    <div class="Nofval h-12 text-center mt-8 text-black font-extrabold">BARS = {num}</div>
</div>
<style>
    @media (min-width: 768px ) and (max-width:1024px){
        nav{
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            grid-template-rows: repeat(2, 1fr);
            column-gap: 3rem;
            row-gap: 1rem;
            padding-bottom: 1.5rem;
        }
        .range{
            grid-area: 2 / 1 / 3 / 2;
        }
        .AlgoVishu{
            grid-area: 1 / 1 / 2 / 3; 
        }
        .hmm{
            grid-area: 2 / 2 / 3 / 3;
        }
    }
    @media (max-width:768px){
        nav{
            padding-bottom: 1.5rem;
            grid-template-rows: repeat(3, 1fr);
            row-gap: 1rem;
        }
        .range{
            grid-area: 3/1/4/2;
        }
        .AlgoVishu{
            grid-area: 1/1/2/2; 
        }
        .hmm{
            grid-area: 2/1/3/2;
        }
    }
    .concern{
        background: red !important;
    }
</style>