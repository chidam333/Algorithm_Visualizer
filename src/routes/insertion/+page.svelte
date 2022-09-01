<script>
    import {delay,num} from "../stores"
    import {flip} from "svelte/animate"
import Layout from "../+layout.svelte";
    $: reactivity($num,$delay)                              
    $: ValArr=[]
    let curlist;
    let curlist1=Array($num).fill(false);
    let i,j,cur,genid,gen2,gen2id
    function * insertionsort(ValArr){
        for(i=1;i<ValArr.length;i++){
            curlist1=Array($num).fill(false);
            cur=ValArr[i]
            curlist[i]=true
            curlist1[i]=true
                for(j=i-1;ValArr[j]>cur && j>=0;j--){
                    ValArr[j+1]=ValArr[j]
                }
            // }
            curlist1[j]=true
            yield ValArr[j+1]=cur
            curlist1=Array($num).fill(false);
        }
    }
    const fliping = (ValArr,delay)=>{
        const gen = insertionsort(ValArr)
        genid = setInterval(()=>{
            console.log(ValArr)
            let {done} = gen.next()
            if(done){
                clearInterval(genid)
            }
        },delay)
    }
    const reactivity = (num,delay)=>{
        clearInterval(genid)
        curlist = Array(num).fill(false)
        curlist[0]=true
        let k = 0
        ValArr = []
        while(k<num){
            let manke = Math.random()*100
            manke = manke>7 ? manke : manke+5
            ValArr.push(manke)
            k++
        }
        fliping(ValArr,delay+1000)
    }
</script>
<main>
    <div class="h11 mt-1 mb-8 mx-auto w-fit text-orange-600 font-mono font-extrabold">InsertionSort visualiser</div>
    <div class="barplot h-96 bg-gray-200 mb-16 mx-12 border-t-4 border-black">
        <ul class="h-full grid grid-flow-col gap-4">
            {#each ValArr as value , k (value)}
                <li class:real={curlist[k]} class:real1={curlist1[k-1]} animate:flip="{{duration:2000}}" class="bg-green-200 border-x-4 border-b-4 border-black" style="height:{value}%"></li>
            {/each}
        </ul>
    </div>
</main>
<style>
    .real{
        background: orange;
    }
    .real1{
        background: red;
    }
</style>
