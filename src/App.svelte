<script lang="ts">
  import { SvelteUIProvider } from '@svelteuidev/core';
  import { TextInput, InputWrapper } from '@svelteuidev/core';
  import { Button } from '@svelteuidev/core';
  import RangeSlider from "svelte-range-slider-pips";
  import axios from 'axios';


    $: lightColor = `hsl(${Math.round(age[0]) - 10}, 65%, 70%)`;
    $: color = `hsl(${Math.round(age[0])}, 63%, 54%)`;
  let loading = false;
  let age = 48;
  let text = "";
  let currentLetter = 0;
  let displayedText = "";
  let interval
  let user = {
    AARP: 'https://www.aarp.org/',
	"Google": "https://www.google.com",
   "Facebook": "https://www.facebook.com",
   "Twitter": "https://twitter.com",
  "Instagram": "https://www.instagram.com",
  "LinkedIn": "https://www.linkedin.com",
  "Youtube": "https://www.youtube.com",
  "Amazon": "https://www.amazon.com",
  "Wikipedia": "https://www.wikipedia.org",
  "Reddit": "https://www.reddit.com",
  "Netflix": "https://www.netflix.com",
  "Spotify": "https://www.spotify.com",
  "WhatsApp": "https://www.whatsapp.com",
  "Gmail": "https://mail.google.com",
  "Yahoo": "https://www.yahoo.com",
  "Outlook": "https://www.outlook.com",
  "Stack Overflow": "https://stackoverflow.com",
  "GitHub": "https://github.com",
  "TikTok": "https://www.tiktok.com",
  "Pinterest": "https://www.pinterest.com",
  "Quora": "https://www.quora.com",
  "Snapchat": "https://www.snapchat.com",
};



  async function startTyping() {
	loading = true;

	await axios.post('https://open-ai-backend-nodejs-dsubnsdb7-or32.vercel.app/answer', { //https://open-ai-backend-nodejs-dsubnsdb7-or32.vercel.app/answer http://localhost:3000/answer
	"question":text,
    "age":age
}).then((response) => {
	console.log(response.data)
	loading = false;
	function splitText(text) {
    let lines = text.split(/\n/);
    let words = [];
    lines.forEach(line => {
      words = words.concat(line.split(" "));
    });
    return words;
}


	let myArray = splitText(response.data);
	myArray = myArray.map(s => s === "" ? "<br/>" : s);

	 interval = setInterval(() => {
        if (currentLetter < myArray.length) {
			if(myArray[currentLetter] in user){
				displayedText +=  ` <a href="${user[myArray[currentLetter]]}" target="_blank">${myArray[currentLetter]}</a> `;
                currentLetter++;
				
			}
			if(myArray[currentLetter] =="\n" ){
				displayedText += ` \n`;
                currentLetter++;

			}else{

                displayedText += ` ${myArray[currentLetter]}`;
                currentLetter++;
				
			}
		
        }else{
			interval = undefined;
			clearInterval(interval);
		}
    }, Math.random() * (300 - 100) +100 );
	
}, (error) => {
  loading = false;
  console.log(error);
});
text =''
displayedText = "";
let currentLetter = 0;

   
  }


  
  
</script>


<main>

	<SvelteUIProvider >
		<h1>explained to a {age} year old</h1>
		<div class="slider" style="--range-handle-focus: {color}; --range-range: {lightColor}">
		<RangeSlider id="color-pips" bind:values={age}  min={5} max={99} step={1} pips  range="min" first={false} last={false} />
	   </div>
	    <div class="input">
			<TextInput  bind:value={text} />
		</div>
	
		{#if !interval && !loading}
		<button class="button-49" role="button" on:click={startTyping}>explain</button>
		{/if}
		{#if loading}
        <div>Loading...</div>
       {:else}
	    <div class="answer-div">
		<p class="answer">{@html displayedText}</p>
	     </div>
        {/if}
		

		
		
	    

		
	</SvelteUIProvider>	  
	{#if interval }
	
	 <button class="button-49 stop" role="button" on:click={()=>{clearInterval(interval); interval = undefined} }>
		stop
	  </button>
    {/if}

</main>







<style >
#color-pips {
  height: 13px;
}
#color-pips .rangeBar {
  height: 13px;
}
#color-pips .rangeHandle {
  top: 7px;
  height: 30px;
  width: 30px;
}
#color-pips .rangeHandle .rangeNub {
  border: 3px solid rgb(246, 252, 255);
}
#color-pips .rangePips {
  bottom: -3px;
  z-index: 1;
}
#color-pips .rangePips .pip {
  background: rgb(246, 252, 255);
  border-radius: 10px;
  width: 5px;
  height: 5px;
  transform: translateX(-50%);
}
#color-pips .rangePips .pip.in-range {
  background: rgb(0, 0, 0);
  opacity: 0.35;
}
	main {
	
		text-align: center;
		padding: 1em;
		margin: 5 auto;
		margin-top: 0em;
		display: block;
        justify-content: space-around;
	
	}
	.slider{
		margin-bottom: 3em;
	}
	.input{
		margin-bottom: 1em;


	}
	.answer-div{
		display: block;
        margin-left: auto;
        margin-right: auto;
		width: 80%;
	
		
	}
	.answer{
		text-align: left;
		justify-content: center;
		max-width: fit-content;
		line-height: 150%;
		
		
	}


	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
		margin-bottom: 1em;
	}
	.stop{
		margin-top: 10em;
	}

	.button-49,
.button-49:after {
  width: 150px;
  height: 76px;
  line-height: 78px;
  font-size: 20px;
  font-family: 'Bebas Neue', sans-serif;
  background: linear-gradient(45deg, transparent 5%, #FF013C 5%);
  border: 0;
  color: #fff;
  letter-spacing: 3px;
  box-shadow: 6px 0px 0px #00E6F6;
  outline: transparent;
  position: relative;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
}

.button-49:after {
  --slice-0: inset(50% 50% 50% 50%);
  --slice-1: inset(80% -6px 0 0);
  --slice-2: inset(50% -6px 30% 0);
  --slice-3: inset(10% -6px 85% 0);
  --slice-4: inset(40% -6px 43% 0);
  --slice-5: inset(80% -6px 5% 0);
  
  content: 'ALTERNATE TEXT';
  display: block;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(45deg, transparent 3%, #00E6F6 3%, #00E6F6 5%, #FF013C 5%);
  text-shadow: -3px -3px 0px #F8F005, 3px 3px 0px #00E6F6;
  clip-path: var(--slice-0);
}

.button-49:hover:after {
  animation: 1s glitch;
  animation-timing-function: steps(2, end);
}

@keyframes glitch {
  0% {
    clip-path: var(--slice-1);
    transform: translate(-20px, -10px);
  }
  10% {
    clip-path: var(--slice-3);
    transform: translate(10px, 10px);
  }
  20% {
    clip-path: var(--slice-1);
    transform: translate(-10px, 10px);
  }
  30% {
    clip-path: var(--slice-3);
    transform: translate(0px, 5px);
  }
  40% {
    clip-path: var(--slice-2);
    transform: translate(-5px, 0px);
  }
  50% {
    clip-path: var(--slice-3);
    transform: translate(5px, 0px);
  }
  60% {
    clip-path: var(--slice-4);
    transform: translate(5px, 10px);
  }
  70% {
    clip-path: var(--slice-2);
    transform: translate(-10px, 10px);
  }
  80% {
    clip-path: var(--slice-5);
    transform: translate(20px, -10px);
  }
  90% {
    clip-path: var(--slice-1);
    transform: translate(-10px, 0px);
  }
  100% {
    clip-path: var(--slice-1);
    transform: translate(0);
  }
}


</style>