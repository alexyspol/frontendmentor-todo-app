<script setup>
	defineProps(['text', 'isChecked'])
</script>

<template>
	<li>
		<label>
			<input type="checkbox" name="todo" @change="$emit('checkboxChange')" :checked="isChecked">

			<div class="fancy-checkbox">
				<img src='../../public/icon-check.svg' alt="">
			</div>

			<p>{{ text }}</p>
			<button @click="$emit('delete')">
				<img src='/icon-cross.svg' alt="">
			</button>
		</label>
	</li>
</template>

<style scoped>
	li:first-child {
		overflow: hidden;
	}

	li {
		border-bottom: 1px solid var(--todo-border);

		&:hover .fancy-checkbox {
			background-image: var(--checkbox-gradient);
		}
	}

	label {
		padding: var(--spacing-block) var(--spacing-inline);
		width: 100%;
		cursor: pointer;
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	input {
		display: none;
	}

	.fancy-checkbox {
		background-color: var(--checkbox);
        border-radius: 50%;
        width: var(--spacing-inline);
        aspect-ratio: 1/1;

        display: flex;
        align-items: center;
        justify-content: center;

		> img {
			display: none;
		}

		&:after {
			content: '';
			width: calc(var(--spacing-inline) - .2rem);
			aspect-ratio: 1/1;
			border-radius: 50%;
			background-color: var(--todo-background);
		}
    }

	:checked {
		~ .fancy-checkbox {
			background-image: var(--checkbox-gradient);

			&:after {
				display: none;
			}
		}
		~ .fancy-checkbox img {
			display: inline;
		}
		~ p {
			text-decoration: line-through;
			color: var(--text-completed);
		}
	}

	p {
		font-weight: 400;
		flex: 1;
		padding-inline: var(--spacing-inline);
        margin: 0;
		line-height: normal;
		color: var(--text);
	}

	button {
		display: flex;
		justify-content: center;
		align-items: center;
		padding: 0;
		border: none;
		background: none;
		cursor: pointer;
	}
</style>
