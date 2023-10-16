<template>
    <main>
        <header>
            <h1>Todo</h1>
            <div class="theme-picker">
                <label>
                    <input type="checkbox" name="theme"
                        :checked="theme === 'light-mode'"
                        @click="onToggleTheme">

                    <img src="/icon-sun.svg" alt="">
                    <img src="/icon-moon.svg" alt="">
                </label>
            </div>
    </main>
</template>

<style scoped>
    main {
        width: 100%;
        max-width: 60rem;
        margin: 75px auto 0;
        padding-inline: var(--spacing-inline);
        color: var(--text);
    }

	header {
		display: grid;
        grid-template-columns: 1fr 1fr;
        grid-template-areas: 
            "title theme"
            "input input";
        margin-bottom: var(--spacing-block);
	}

	h1 {
        margin: 0;
		font-weight: 700;
		text-transform: uppercase;
		color: white;
        grid-area: title;
	}

    .theme-picker {
        grid-area: theme;
        text-align: right;

        & input,
        & input:checked ~ img[src*='sun'],
        & input:not(:checked) ~ img[src*='moon'] {
            display: none;
        }
    }

    label {
        cursor: pointer;
    }

	label:focus-visible,
	label:focus {
		outline: 1px solid red;
	}

