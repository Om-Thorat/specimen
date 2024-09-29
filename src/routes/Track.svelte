<script lang='ts'>
    import Card from "./Card.svelte";
    import { onMount } from "svelte";
    let track:HTMLDivElement;
    let y = 0;

    function arrange(){
        const cards = Array.from(track.children) as Array<HTMLDivElement>;
        cards.forEach((card, index) => {
            card.style.left =  `${-100/7 + index * 100/7}%`;
            card.style.top = `${112.5 - (index+1) * 12.5}%`;

            card.style.filter = `blur(${Math.abs(index+1 - 5)}px)`
        });
    }

    function move(e:any){
        console.log(e)
        // if(e.deltaY > 50){
        const cards = Array.from(track.children) as Array<HTMLDivElement>;
        cards.sort((a, b) => {
            const leftA = parseFloat(a.style.left);
            const leftB = parseFloat(b.style.left);
            return leftA - leftB;
        });

        cards.forEach((card, index) => {
            card.style.left =  `${-100/7 + (index+1) * 100/7}%`;
            card.style.top = `${112.5 - (index+1+1) * 12.5}%`;
            card.style.zIndex = `${index+1}`;
            card.style.filter = `blur(${Math.abs(index+1+1 - 5)}px)`
        });
        const lastCard = track.children[0].cloneNode(true) as HTMLDivElement;
        lastCard.style.left = `${-200/7}%`;
        lastCard.style.zIndex = `0`;
        lastCard.style.top = `${112.5}%`;
        lastCard.style.filter = `blur(${Math.abs(1 - 6)}px)`;
        track.removeChild(cards.pop()); 
        track.appendChild(lastCard);
        // lastCard.style.left = `${-100/7}%`;
        // lastCard.style.top = `${100}%`;
        // lastCard.style.filter = `blur(${Math.abs(1 - 5)}px)`;
        // arrange();     
    // }

}

    onMount(() => {
        arrange();
        // window.addEventListener('wheel', move);
        window.onclick = move;
    });
</script>

<svelte:window bind:scrollY={y} />

<div class="w-full max-h-[100svh] min-h-[100svh] overflow-hidden absolute top-0 " bind:this={track}>

    {#each Array(9) as _, index}
        <Card key={index} />
    {/each}

</div>