<script>
    import { onMount } from "svelte";
    import {page} from "$app/stores"
    import "../app.css";
    import {flip} from "svelte/animate"
    let bubblesort=false
    const rerun = () => {
        if($page.url.pathname!="/"){
            console.log($page.url.pathname)
            bubblesort=true}
        else{
            bubblesort=false}
        }
    let on= [true,false,false,false,false]
    let delay=1000
    let genid;
    let ValArr = []
    let StateArr = []
    let inde=0
    let nexinde=1
    let num = 7;
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
    const flippinsortin = (ValArr)=>{
                const gen = bubbleSort(ValArr)
                genid = setInterval(()=>{
                    const {done}=gen.next();
                    if(done){
                        clearInterval(genid)
                    }
                },delay)
    }
    const reactive= (num)=>{
        clearInterval(genid)
        let fliptime=delay
        if(delay<200){
            fliptime=2000
        }
        let i = 0;
        ValArr=[]
        while (i<num) {
            ValArr.push(Math.random()*100)
            i+=1
        }
        StateArr = Array(num).fill(false)
        flippinsortin(ValArr)
    }
    function delaychangehandle(quant){
        delay=1000 - quant*200
        reactive(num)
        on=Array(5).fill(false)
        on[quant]=true
    }
    onMount(()=>{reactive(num)})

</script>
<nav class="nav w-full lg:h-20 bg-orange-700 grid lg:grid-flow-col lg:gap-12 px-12">
    <div class="range h-full grid items-center">
        <label for="r" class="font-extrabold text-lime-200 grid justify-center">No. of values ( 5 - 35 )</label>
        <input type="range" id="r" min="5" max="35" class="w-full" bind:value={num} on:input={reactive(num)}>
    </div>
    <div class="AlgoVishu flex lg:flex-col 1 -skew-x-12 mx-auto text-lime-200 font-extrabold tracking-widest"><span class="lg:-ml-8">Sorting &nbsp;</span><span>Algorithm &nbsp;</span><span class="lg:ml-8">Visualiser</span></div>    
    <div class="hmm h-full grid grid-flow-col p-4">
    {#each Array(5) as i,k (k)}
        <button class:on={on[k]} class="bg-lime-300 border-4 border-black font-extrabold" on:click={()=>{delaychangehandle(k)}}>{k+1}x</button>     
    {/each}
    </div>
</nav>
<nav>
<div class="choose h-8 w-screen grid grid-flow-col mt-4 lg:px-36 overflow-hidden">
    <a on:click={()=>{rerun()}}  href="/" class="mr-2 bg-zinc-900 text-center border-x-4 border-cyan-100 -skew-x-12 text-cyan-50">Bubble sort</a >
    <a on:click={()=>{rerun()}} href="/insertion" class="btn mr-2 text-center bg-zinc-900 h-full border-x-4 border-cyan-100 -skew-x-12 text-cyan-50">Insertion sort</a>
    <a on:click={()=>{rerun()}} href="/mergesort" class="btn text-center mr-2 bg-zinc-900 h-full border-x-4 border-cyan-100 -skew-x-12 text-cyan-50">Merge sort</a>
    <a on:click={()=>{rerun()}} href="quicksort" class="btn text-center mr-2 bg-zinc-900 h-full border-x-4 border-cyan-100 -skew-x-12 text-cyan-50">Quick Sort</a>
</div>
</nav>
<br>
<slot />
<div class:other={bubblesort} class="barplotarea  mx-12 h-96 mb-12 border-t-4 border-black bg-gray-200">
    <ul class="w-full h-full grid grid-flow-col lg:gap-4 sm:gap-0">
        {#each ValArr as i,k (i) }
            <li class:concern={StateArr[k]} animate:flip="{{duration:delay}}" class="border-x-4 border-b-4 border-black bg-lime-400" style="height: {i}%;" ></li>
        {/each}
    </ul>
    <div class="Nofval h-12 text-center mt-8 text-black font-extrabold">BARS = {num}</div>
</div>

<style>
    .other{
        display: none;
    }
    @media (min-width: 768px ) and (max-width:1024px){
        .nav{
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
        .nav{
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
    .on{
        color: wheat;
        background: rgb(97, 40, 0);
    }
</style>