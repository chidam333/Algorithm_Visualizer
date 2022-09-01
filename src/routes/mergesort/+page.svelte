<script>
    import {num,delay} from "../stores"
    let [ValArr,sArr,resu] = [[],[],[],[],[]]
    let i,id,result,step=-1;
    $: $num=6
    $: yeah=($num>=7) ? false : true
    $: reactive(6),$num
    let okArr
    okArr=Array(0).fill(false)
    function mergesort(ValArr){
        step+=1
        okArr.push(false)
        sArr[step]=ValArr
        const len = ValArr.length
        if(len<=1){
            return ValArr
        }
        const mid = Math.floor(len/2)
        const left = ValArr.slice(0,mid)
        const right = ValArr.slice(mid)
        return merge(mergesort(left),mergesort(right))
    }
    function merge(left,right){
        let k
        console.log("Left is : ",left,"Right is : ",right)
        for(k=0;k<=step;k++){
            if(sArr[k]==left || sArr[k]==right){
                    // Array(step).fill(false)
                okArr[k]=true
            }
        }
        let lind=0,rind=0,Res=0;
        while(lind<left.length && rind<right.length){
            if(left[lind]<right[rind]){
                ValArr[Res]=left[lind]
                lind++
                Res++
            }
            else{
                ValArr[Res]=right[rind]
                Res++                
                rind++
            }
        }
        ValArr = ValArr.slice(0,Res)
        const resultant = ValArr.concat(left.slice(lind)).concat(right.slice(rind))
        resu.push(resultant)
        return resultant

    }
 
    const reactive = (num) =>{
        [ValArr,sArr] = [[],[]]
        step=-1
        for(i=0;i<num;i++){
            let manke = Math.random()*100
            manke = (manke>7) ? manke : manke + 7
            ValArr.push(manke)
        }
        mergesort(ValArr)
    }
</script>
<main>
    <div class="small h-72 mb-16 bg-black text-white font-extrabold grid place-items-center lg:hidden">
        still making it for small devices
    </div>
    <div class="full hidden lg:block">
    <div class="h11 mt-1 mb-8 mx-auto w-fit text-orange-600 font-mono font-extrabold">Mergesort visualiser</div>
    <div class="alert bg-red-400 mx-12 h-16 mb-4 p-0 relative" class:yeah={yeah}>
        <div class="close absolute right-4 font-extrabold border-2 border-red-900 mt-2 px-2 cursor-pointer" on:click={()=>{yeah=true}}>X</div>
        <div class="alertmsg font-extrabold text-red-900 absolute left-12 top-4">To make visualisation possible it is restricted to 6 numbers for now !!</div>
    </div>
    <div class="stuff flex">
    <div class="lol h-96 bg-gray-200 overflow-scroll overflow-x-hidden ml-12 pl-4 pt-4 pb-2 mb-12 w-1/2">
        {#each sArr as arr,i}
        <div class="01 flex">
            {#each arr as value}
                <div class="val mr-4 border-4 border-black mb-2 p-2 bg-green-300" class:merge={okArr[i]}>{Math.floor(value)}</div>
            {/each}
        </div>
        {/each}
    </div>
    <div class="other h-96 w-1/2 overflow-scroll border-2 border-black ml-4 mr-12">
        <div class="head text-center font-extrabold">Merging</div>
        {#each resu as arr}
        <div class="flex">
            {#each arr as value}
                <div class="mr-2 border-4 border-black ml-4 my-2 p-2">&nbsp;{Math.floor(value)}</div>
            {/each}
        </div>
        {/each}
    </div>
    </div>
    </div>
</main>
<style>
    .yeah{
        display:none;
    }
    .merge{
        background: red;
    }
    .lol::-webkit-scrollbar-track{
        background: black;
        border-radius: 17px;
    }
    .lol::-webkit-scrollbar-thumb{
        background: orangered;
        border-radius: 17px;
    }
    .lol::-webkit-scrollbar-corner{
        background: orangered;
    }
    .lol::-webkit-scrollbar{
        width: 10px;
    }
</style>
