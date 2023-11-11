<script lang="ts">
	import { Confetti } from 'svelte-confetti';
	import { animate } from 'motion';

	let step = 1;

	function nextStep() {
		if (step == 1) {
			step = 2;
			setTimeout(() => {
				step = 3;
			}, 3000);
		} else if (step == 3) {
			step = 4;
		}
	}

	let quiz = [
		{
			question: 'Vad heter världens bästa pappa?',
			answers: [
				{
					text: 'Lars-Fritjof',
					correct: false
				},
				{
					text: 'André',
					correct: true
				},
				{
					text: 'Greger',
					correct: false
				},
				{
					text: 'Klas-Niklas',
					correct: false
				}
			]
		},
		{
			question: 'Är du en pappgubbe?',
			answers: [
				{
					text: 'Ja såklart',
					correct: true
				},
				{
					text: 'Nej för fan',
					correct: false
				},
				{
					text: 'Jomen det kan hända',
					correct: true
				},
				{
					text: 'Tvek på den asså',
					correct: false
				}
			]
		},
		{
			question: 'Är du en kärleksfull fader?',
			answers: [
				{
					text: 'Jamen visst!',
					correct: false
				},
				{
					text: 'Nejjj, jag är ju en kä-fu-fa',
					correct: true
				},
				{
					text: 'Alltså, jag vet faktiskt inte',
					correct: false
				},
				{
					text: 'Kom inte på ett till svar :(',
					correct: false
				}
			]
		},
		{
			question: 'Hur många turkar bor bodde i Azerbaijan år 2009?',
			answers: [
				{
					text: '2',
					correct: false
				},
				{
					text: 'Typ 1 mille ish',
					correct: false
				},
				{
					text: '38000',
					correct: true
				},
				{
					text: 'För många för att ens räkna',
					correct: false
				}
			]
		}
	];

	let question = 0;
	let correct = 0;

	function submitAnswer(answer: { text: string; correct: boolean }) {
		if (answer.correct) {
			correct++;
		}
		question++;
	}

	function resetQuiz() {
		question = 0;
		correct = 0;
	}

	let counter = 101;

	let degrees = 0;

	function decrementCounter() {
		counter--;
		if (counter >= 80 && counter < 90) {
			const height = document.documentElement.clientHeight;
			const width = document.documentElement.clientWidth;
			let randY = Math.floor(Math.random() * (height - width / 2) + 1);
			let randX = Math.floor((Math.random() * width) / 2 + 1);
			giftStyle = `transform: translate(${randX}px, ${randY}px); position: absolute; top: 0; left: 0`;
		} else if (counter >= 60 && counter < 70) {
			giftStyle = 'scale: 0.01;';
		} else if (counter >= 41 && counter < 50) {
			counter += 0.5;
		} else if (counter >= 20 && counter < 30) {
			degrees += 45;
			giftStyle = `rotate: ${degrees}deg;`;
		} else if (counter > 0 && counter < 10) {
			hideAndShowEnabled = true;
		} else if (counter <= 0) {
			hideAndShowEnabled = false;
			step = 5;
			setTimeout(() => {
				animate(
					'.gift',
					{ rotate: [-15, 15] },
					{
						duration: 0.3,
						easing: 'ease-in-out',
						repeat: 10
						//direction: 'alternate'
					}
				);
			}, 0);
			setTimeout(() => {
				step = 6;
				setTimeout(() => {
					giftCardInvisible = false;
					animate(
						'.giftcard',
						{ scale: [0, 1] },
						{
							easing: 'ease-in',
							duration: 20
						}
					);
				}, 0);
			}, 3000);
		} else {
			counter = Math.round(counter);
			giftStyle = '';
		}
	}

	let hideAndShowEnabled = false;

	$: if (hideAndShowEnabled) {
		hideAndShow();
	}
	function hideAndShow() {
		giftStyle = 'visibility: hidden;';
		setTimeout(() => {
			giftStyle = '';
		}, 1000);
		setTimeout(() => {
			if (hideAndShowEnabled) {
				hideAndShow();
			}
		}, 2000);
	}

	let giftStyle = '';
	let giftCardInvisible = true;
</script>

<div
	style="position: fixed; top: -50px; left: 0; height: 100vh; width: 100vw; display: flex; justify-content: center; overflow: hidden; pointer-events: none;"
>
	<Confetti
		x={[-5, 5]}
		y={[0, 0.1]}
		delay={[500, 2000]}
		infinite
		duration={5000}
		amount={400}
		fallDistance="100vh"
	/>
</div>
<div class="flex flex-col justify-center items-center h-[100svh]">
	{#if step == 1}
		<p class="font-bold text-xl">Grattis på Fars Dag!! 🎉🎉🎉🎉</p>
		<p>Här har du en present:</p>
		<button class="mt-8 w-1/2" on:click={nextStep}>
			<img src="giftbox.png" alt="gift" class=" " />
		</button>
	{:else if step == 2}
		<p class="font-bold text-lg">Nae, så lätt var det inte sörru!</p>
		<p>Nu är det Quiz-time 🔥🔥🔥</p>
	{:else if step == 3}
		{#if question < quiz.length}
			<p class="font-bold text-xl p-4 text-center">{quiz[question].question}</p>
			<div class="grid grid-rows-2 p-4 grid-cols-2 gap-2 w-full">
				{#each quiz[question].answers as answer (answer.text)}
					<button
						class="rounded-xl text-lg bg-stone-800 p-4 flex items-center justify-center shadow-xl"
						on:click={() => submitAnswer(answer)}
					>
						{answer.text}
					</button>
				{/each}
			</div>
		{:else}
			<p class="font-bold text-xl p-4 text-center">Du fick {correct} av {quiz.length} rätt!</p>
			{#if correct == quiz.length}
				<p class="text-center">Eyy, kingen! Alla rätt 🎉🎉</p>
				<p>Detta betyder ju att du är världens bästa pappa! ❤</p>
				<button class="bg-green-500 rounded-xl p-2 shadow-xl mt-4 font-bold" on:click={nextStep}
					>Öppna presenten</button
				>
			{:else}
				<p class="text-center font-semibold p-4">
					Oj vad steelt! Du måste ju få alla rätt för att öppna din present... 🙄🙄
				</p>
				<button class="bg-stone-800 rounded-xl p-2 shadow-xl mt-4" on:click={resetQuiz}
					>Men du kan ju alltid köra igen :)</button
				>
			{/if}
		{/if}
	{:else if step == 4}
		<p class="font-bold text-xl">Varsågod!</p>
		<p>Tryck på presenten för att öppna den:</p>
		<p class:invisible={counter > 100}>{counter}</p>
		<button class="mt-8 w-1/2 transition-transform" style={giftStyle} on:click={decrementCounter}>
			<img src="giftbox.png" alt="gift" class=" " />
		</button>
	{:else if step == 5}
		<p class="font-bold text-xl">Öppnar presenten...</p>
		<div class="mt-8 w-1/2 gift">
			<img src="giftbox.png" alt="gift" class="" />
		</div>
	{:else if step == 6}
		<p class="font-bold text-xl">Grattis på fars dag pappa! ❤</p>
		<div class="mt-8 w-2/3 giftcard relative" class:invisible={giftCardInvisible}>
			<img src="giftcard.png" alt="giftcard" class="" />
			<p class="absolute bottom-4 right-4 font-bold text-xl z-20">200kr</p>
		</div>
	{/if}
</div>
