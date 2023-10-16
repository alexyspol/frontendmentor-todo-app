<script setup>
    import { ref, watch, computed, onMounted } from 'vue';
    // 1: Toggle Theme

    let theme = ref('light-mode');

    function onToggleTheme() {
        theme.value = theme.value === 'light-mode' ? 'dark-mode' : 'light-mode';
    }

    watch(theme, (newTheme) => {
        document.body.dataset.theme = newTheme;
    });

    document.body.dataset.theme = theme.value;

    // 2: Create / Delete / Filter Todos

    const todos = ref([]);
    const rAddTodo = ref(null);
    function onAddTodo() {
        todos.value = [...todos.value, {
            text: rAddTodo.value.value,
            completed: false,
            isHidden: false
        }];
        rAddTodo.value.value = '';
    }

</script>

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

            <div class="add-todo">
                <span class="dummy-checkbox"></span>
                <input
                    type="text"
                    placeholder="Create a new todo..."
                    @keyup.enter="onAddTodo"
                    ref="rAddTodo" />
            </div>
        </header>
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

    .add-todo {
        grid-area: input;
        padding: var(--spacing-block) var(--spacing-inline);
        margin-top: 40px;
        background-color: var(--todo-background);
        border-radius: var(--rounded-corners);
        display: flex;
        cursor: pointer;

        & input {
            flex: 1;
            margin-left: 10px;
            border: none;
            font-family: inherit;
            font-size: inherit;
            outline: none;
            color: inherit;
            caret-color: var(--primary);
            background: none;

            &::placeholder {
                color: var(--text-placeholder);
            }
        }
    }

    .add-todo:hover .dummy-checkbox,
    .dummy-checkbox:has(+ :focus) {
        background-image: var(--checkbox-gradient);
    }

	.dummy-checkbox {
		background-color: var(--checkbox);
        border-radius: 50%;
        width: 20px;
        aspect-ratio: 1/1;

        display: flex;
        align-items: center;
        justify-content: center;

		&:after {
			content: '';
			width: 18px;
			aspect-ratio: 1/1;
			border-radius: 50%;
			background-color: var(--todo-background);
		}
    }

