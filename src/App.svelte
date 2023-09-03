<script>
	import Card from "./Card.svelte";
	let front;
	let back;
	let group;

	let cardsList = [
		{
			groupName: "add a new card",
			front: "this is the front of a card",
			back: "this is the back of a card",
			completed: false,
		},
	];
	let currentGroup = "add a new card";

	let cardContainer = document.querySelector("#card-container");
	let filteredcardsList;
	let card;
	function addOneCard() {
		cardContainer = document.querySelector("#card-container");
		filteredcardsList = cardsList.filter(
			(card) => card.completed === false && card.groupName == currentGroup
		);
		card = filteredcardsList[0];
		const cardCompoent = new Card({
			target: cardContainer,
			props: {
				front: card.front,
				back: card.back,
				completed: card.completed,
			},
		});
		cardCompoent.$on("done", () => {
			for (let i = 0; i < cardsList.length - 1; i++) {
				if (
					cardsList[i].groupName === card.groupName &&
					cardsList[i].front === card.front &&
					cardsList[i].back === card.back
				) {
					cardsList[i].completed = true;
				}
			}
			addOneCardCopy();
			cardCompoent.$destroy();
		});
	}

	function addOneCardCopy() {
		cardContainer = document.querySelector("#card-container");
		filteredcardsList = cardsList.filter(
			(card) => card.completed === false && card.groupName == currentGroup
		);
		card = filteredcardsList[0];
		const cardCompoent = new Card({
			target: cardContainer,
			props: {
				front: card.front,
				back: card.back,
				completed: card.completed,
			},
		});
		cardCompoent.$on("done", () => {
			for (let i = 0; i < cardsList.length - 1; i++) {
				if (
					cardsList[i].groupName === card.groupName &&
					cardsList[i].front === card.front &&
					cardsList[i].back === card.back
				) {
					cardsList[i].completed = true;
				}
			}
			cardCompoent.$destroy();
		});
	}
	let innerGroupSelection;
	function addCardForm() {
		if (group != "current") {
			cardsList.push({
				groupName: group,
				front: front,
				back: back,
				completed: false,
			});
			const uniqueGroups = new Set();
			cardsList.forEach((card) => {
				uniqueGroups.add(card.groupName);
			});
			groups = Array.from(uniqueGroups);
			innerGroupSelection = "";
			groups.forEach((group) => {
				innerGroupSelection += `<option value="${group}">${group}</option>`;
				console.log(innerGroupSelection);
			});
			document.querySelector("#groupSelection").innerHTML =
				innerGroupSelection;
		} else {
			cardsList.push({
				groupName: currentGroup,
				front: front,
				back: back,
				completed: false,
			});
			const uniqueGroups = new Set();
			cardsList.forEach((card) => {
				uniqueGroups.add(card.groupName);
			});
			groups = Array.from(uniqueGroups);
			innerGroupSelection = "";
			groups.forEach((group) => {
				innerGroupSelection += `<option value="${group}">${group}</option>`;
				console.log(innerGroupSelection);
			});
			document.querySelector("#groupSelection").innerHTML =
				innerGroupSelection;
		}
	}
	let groups = [];

	window.addEventListener("DOMContentLoaded", () => {
		addOneCard();
		const uniqueGroups = new Set();
		cardsList.forEach((card) => {
			uniqueGroups.add(card.groupName);
		});
		groups = Array.from(uniqueGroups);
	});
</script>

<main>
	<div id="card-container" />
	<div id="form">
		<div>
			<select bind:value={currentGroup} id="groupSelection">
				{#each groups as group}
					<option value={group}>{group}</option>
				{/each}
			</select>
			<div>
				<input
					type="text"
					bind:value={front}
					placeholder="front of card"
				/>
				<input
					type="text"
					bind:value={back}
					placeholder="back of card"
				/>
				<input
					type="text"
					bind:value={group}
					placeholder="group of card"
				/>
				<button on:click={addCardForm}>Done</button>
			</div>
		</div>
	</div>
</main>

<style>
	#form {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	button {
		user-select: none;
	}
</style>
