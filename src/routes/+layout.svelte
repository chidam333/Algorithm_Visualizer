<script>
    import "../app.css";
    import {delay,num} from "./stores"    
    let on= [true,false,false,false,false]
    function delaychangehandle(quant){
        $delay=1000 - quant*200
        console.log(on)
        on=Array(5).fill(false)
        on[quant]=true
    }

</script>
<nav class="nav w-full lg:h-20 bg-orange-700 grid lg:grid-flow-col lg:gap-12 px-12">
    <div class="range h-full grid items-center">
        <label for="r" class="font-extrabold text-lime-200 grid justify-center">No. of values ( 5 - 35 )</label>
        <input type="range" id="r" min="5" max="35" class="w-full" bind:value={$num}>
    </div>
    <div class="AlgoVishu flex lg:flex-col 1 -skew-x-12 mx-auto text-lime-200 font-extrabold tracking-widest"><span class="lg:-ml-8">Sorting &nbsp;</span><span>Algorithm &nbsp;</span><span class="lg:ml-8">Visualiser</span></div>    
    <div class="hmm h-full grid grid-flow-col p-4">
    {#each Array(5) as i,k (k)}
        <button class:on={on[k]} class="bg-lime-300 border-4 border-black font-extrabold" on:click={()=>{delaychangehandle(k)}}>{k+1}x</button>     
    {/each}
    </div>
</nav>
<nav>
<div class="choose h-8 w-[100%] grid grid-flow-col mt-4 lg:px-36 overflow-hidden">
    <a  href="/" class="mr-2 bg-zinc-900 text-center border-x-4 border-cyan-100 -skew-x-12 text-cyan-50">Bubble sort</a >
    <a href="/insertion" class="btn mr-2 text-center bg-zinc-900 h-full border-x-4 border-cyan-100 -skew-x-12 text-cyan-50">Insertion sort</a>
    <a href="/mergesort" class="btn text-center mr-2 bg-zinc-900 h-full border-x-4 border-cyan-100 -skew-x-12 text-cyan-50">Merge sort</a>
</div>
</nav>
<br>
<div class="slot"><slot /></div>
<footer>
    <div class="foot w-[100%] h-20 bg-black">
        <div class="name mx-auto w-fit pt-6 text-yellow-200 hover:text-blue-300 font-extrabold"><a href="https://twitter.com/Chidam333" target="_blank">@chidam333</a></div>
    </div>
</footer>
<style>
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

    .on{
        color: wheat;
        background: rgb(97, 40, 0);
    }
</style>
