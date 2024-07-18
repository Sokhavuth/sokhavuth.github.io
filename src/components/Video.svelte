<script>
import { onMount } from "svelte"
export let videos, title
let clicked = 0
let iframe = ''
let myElement

function setScreen(video,id,click){
    if(video['vidType'] == 'OK'){
        var url = `//ok.ru/videoembed/${video['id']}`
    }else if(video['vidType'] == 'YouTube'){
        var url = `https://www.youtube.com/embed/${video['id']}`
    }else if(video['vidType'] == 'YouTubePlaylist'){
        var url = `https://www.youtube.com/embed/videoseries?list=${video['id']}`
    }else if(video['vidType'] === "Facebook"){
        var url = `https://www.facebook.com/watch/?v=${video['id']}`
    }else if(video['vidType'] === "GoogleDrive"){
        var url = `https://docs.google.com/file/d/${video['id']}/preview`
    }else if(video['vidType'] === "Vimeo"){
        var url = `https://player.vimeo.com/video/${video['id']}`
    }else if(video['vidType'] === "Dailymotion"){
        var url = `https://www.dailymotion.com/embed/video/${video['id']}`
    }

    if(video['vidType'] !== 'Facebook'){
        iframe = `<div><iframe  src="${url}" frameborder="0" allowfullscreen></iframe></div>`
    }else{
        iframe = `<div class="fb-video" data-href="${url}" data-width="auto" data-show-captions="true"></div>`
    }
    
    if(click){
        myElement = document.getElementById("part"+clicked)
        myElement.classList.remove('clickedPart')
    }

    myElement = document.getElementById("part"+id)
    myElement.classList.add('clickedPart')
    
    clicked = id
}

onMount(() => {
    setScreen(videos[0],0,false)
})
</script>

{#if videos}
<div class="video">
    <div class="screen">
        {@html iframe }
    </div>
    <div class="playlist">
    {#each videos as item, index }
        {#if index === videos.length-1}
        <div id="part{index}"  on:click={() => setScreen(videos[index], index,true)} class="part">
            { title } ភាគ { index+1 } { videos[index].status }
        </div>
        {:else}
        <div id="part{index}" on:click={() => setScreen(videos[index], index,true)} class="part">
            { title } ភាគ { index+1 }
        </div>
        {/if}
    {/each}
    </div>
</div>
{/if}