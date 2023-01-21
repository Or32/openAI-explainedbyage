<script lang="ts">
  import { SvelteUIProvider } from '@svelteuidev/core';
  import { TextInput, InputWrapper } from '@svelteuidev/core';
  import { Button } from '@svelteuidev/core';
  import RangeSlider from "svelte-range-slider-pips";
  import axios from 'axios';


    $: lightColor = `hsl(${Math.round(age[0]) - 10}, 65%, 70%)`;
    $: color = `hsl(${Math.round(age[0])}, 63%, 54%)`;

  let age = 48;
  let text = "";
  let currentLetter = 0;
  let displayedText = "";
  let interval


  async function startTyping() {

	

	await axios.post('https://open-ia-backend-nodejs-lf9k.vercel.app/answer', {
	"question":text,
    "age":age
}).then((response) => {
	 interval = setInterval(() => {
		
		
        if (currentLetter < response.data.length) {
            displayedText += response.data[currentLetter];
            currentLetter++;
        }
    }, 100);
	
}, (error) => {
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
	
		{#if !interval}
		<button class="button-49" role="button" on:click={startTyping}>explain</button>
		{/if}
	<p>{displayedText}</p>

		
	</SvelteUIProvider>	  
	{#if interval}
	
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
		margin-bottom: 3em;


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