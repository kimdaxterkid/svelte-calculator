<script lang="ts">
	const numbers = ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0", "."];
	const operations = ["/", "x", "-", "+", "="];
	let selectedOperation = "";
	let display = "";
	let firstNumber = "";
	let secondNumber = "";
	let isDisplayingResult = false;

	const handleOpertaionClick = (operation: string) => {
		if (!firstNumber) return;
		if (operation === "=") {
			if (!secondNumber) return;
			const firstNum = parseInt(firstNumber);
			const secondNum = parseInt(secondNumber);
			let result = "";
			switch (selectedOperation) {
				case "/":
					result = (firstNum / secondNum).toFixed(2);
					break;
				case "x":
					result = (firstNum * secondNum).toFixed(2);
					break;
				case "+":
					result = (firstNum + secondNum).toFixed(2);
					break;
				case "-":
					result = (firstNum - secondNum).toFixed(2);
					break;
			}
			display = result;
			isDisplayingResult = true;
		}
		selectedOperation = operation;
	};

	const handleClear = () => {
		firstNumber = "";
		secondNumber = "";
		selectedOperation = "";
		display = "";
		isDisplayingResult = false;
	};

	const handleNumberClick = (number: string) => {
		if (isDisplayingResult) {
			handleClear();
		}
		if (display === "" && number === "0") return;
		if (number === "." && display.includes(".")) return;

		if (!selectedOperation) {
			if (display === "" && number === ".") {
				display = "0.";
				firstNumber = "0.";
				return display;
			}
			firstNumber = `${firstNumber}${number}`;
			display = firstNumber;
			return display;
		} else {
			if (display === "" && number === ".") {
				display = "0.";
				secondNumber = "0.";
				return display;
			}
			secondNumber = `${secondNumber}${number}`;
			display = secondNumber;
			return display;
		}
	};
</script>

<main>
	<div class="calculator">
		<div class="results">
			{display}
		</div>
		<div class="digits">
			<div class="numbers">
				<button class="btn btn-xlg" on:click={() => handleClear()}> C </button>
				{#each numbers as number (number)}
					<button
						class={`btn ${number === "0" ? "btn-lg" : null}`}
						on:click={() => handleNumberClick(number)}
					>
						{number}
					</button>
				{/each}
			</div>
			<div class="operations">
				{#each operations as operation (operation)}
					<button
						class={`btn ${
							operation === selectedOperation ? "btn-silver" : "btn-orange"
						}`}
						on:click={() => handleOpertaionClick(operation)}
					>
						{operation}
					</button>
				{/each}
			</div>
		</div>
	</div>
</main>

<style>
	main {
		width: 100vw;
		height: 100vh;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.calculator {
		background-color: rgb(28, 28, 28);
		width: 240px;
		padding: 15px;
		border-radius: 7px;
	}
	.digits {
		display: flex;
	}
	.operations {
		display: flex;
		flex-direction: column;
	}
	.numbers {
		display: flex;
		flex-wrap: wrap;
		width: 200px;
	}
	.btn {
		width: 50px;
		height: 50px;
		border-radius: 100px;
		background-color: rgb(114, 113, 113);
		font-size: 20px;
		font-weight: bold;
		color: white;
		margin: 5px;
		border: none;
	}
	.btn-lg {
		width: 110px;
	}
	.btn-orange {
		background-color: orange;
	}
	.btn-silver {
		background-color: silver;
	}
	.btn-xlg {
		width: 170px;
	}
	.results {
		height: 50px;
		color: white;
		font-size: 50px;
		display: flex;
		flex-direction: row-reverse;
		margin-right: 10px;
	}
</style>
