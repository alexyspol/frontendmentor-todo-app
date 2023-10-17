<script setup>
    import { ref, watch, computed, onMounted } from 'vue';
    import TodoItem from './TodoItem.vue';
    import AppFooter from './AppFooter.vue';

    // 1: Toggle Theme

    let theme = ref('');

    function onToggleTheme() {
        const newTheme = theme.value === 'light-mode' ? 'dark-mode' : 'light-mode';
        theme.value = newTheme;
        document.body.dataset.theme = newTheme;
        localStorage.setItem('theme', newTheme);
    }

    // 2: Create / Delete / Filter Todos

    const todos = ref([]);
    const rAddTodo = ref(null);
    const filterBy = ref('all');

    const quotes = [
        'It was not the feeling of <b>completeness</b> I so needed, but the feeling of not being empty.',
        '<b>Emptiness</b> is a door to fullness.',
        'In <b>emptiness</b>, there is the potential for everything.',
        '<b>Emptiness</b> doesn\'t mean absence. It means readiness to be filled.',
        '<b>Emptiness</b> is a good fasting place, but a terrible house guest.',
        '<b>Emptiness</b> is a canvas to paint your thoughts.',
        'The void holds the space for something beautiful.',
        '<b>Empty</b> hands can hold infinite possibilities.',
        '<b>Empty</b> spaces create room for new beginnings.',
        'The absence of things can be a presence of peace.',
        '<b>Emptiness</b> allows the heart to breathe freely.'
    ];
    
    const message = computed(() => {
        let result;
        
        const countCompleted = todos.value.reduce((acc, todo) => {
            return todo.completed ? acc + 1 : acc;
        }, 0);
        
        if(['all', 'active'].includes(filterBy.value)) {
            const count = todos.value.length - countCompleted;
            result = `${count} ${count === 1 ? 'item' : 'items'} left`;
        }
        else if(filterBy.value === 'completed') {
            result = `${countCompleted}/${todos.value.length} ${countCompleted === 1 ? 'item' : 'items'} completed`;
        }
        
        return result;
    });

    const emptyListMessage = computed(() => {
        return todos.value.length === 0 ? quotes[Math.floor(Math.random() * quotes.length)] : message.value;
    });
    
    function onAddTodo() {
        todos.value = [...todos.value, {
            text: rAddTodo.value.value,
            completed: false,
            isHidden: false
        }];
        rAddTodo.value.value = '';
    }

    function onDeleteTodo(indexToRemove) {
        todos.value = [...todos.value.slice(0, indexToRemove), ...todos.value.slice(indexToRemove + 1)];
    }

    function onCheckboxChange(indexChecked) {
        todos.value = todos.value.map((item, i) => {
            if(i === indexChecked) {
                return { ...item, completed: !item.completed };
            }
            return item;
        });
    }

    function onClearCompleted() {
        todos.value = todos.value.filter((item) => !item.completed);
        filterBy.value = filterBy.value;
    }

    function onFilterChange(filter) {
        filterBy.value = filter;

        if(filter === 'all') {
            todos.value = todos.value.map(todo => {
                return { ...todo, isHidden: false };
            });
        }
        else if(filter === 'active') {
            todos.value = todos.value.map(todo => {
                todo.isHidden = !todo.completed ? false : true;
                return todo;
            });
        }
        else if(filter === 'completed') {
            todos.value = todos.value.map(todo => {
                todo.isHidden = todo.completed ? false : true;
                return todo;
            });
        }
    }

    // 3. Local Storage

    watch(todos, () => {
        localStorage.setItem('todos', JSON.stringify(todos.value));
    });

    onMounted(() => {
        let data = JSON.parse(localStorage.getItem('todos'));
        todos.value = data.map((eachTodo) => {
            eachTodo.isHidden = false;
            return eachTodo;
        });

        data = localStorage.getItem('theme');
        theme.value = data === '' ? 'light-mode' : data;
        document.body.dataset.theme = theme.value;
    });
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

                    <img src="/frontendmentor-todo-app/icon-sun.svg" alt="">
                    <img src="/frontendmentor-todo-app/icon-moon.svg" alt="">
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
        <article>
            <ol>
                <template v-for="todo, i in todos">
                    <TodoItem
                        v-if="!todo.isHidden"
                        :key="i"
                        :text="todo.text"
                        :isChecked="todo.completed"
                        @checkbox-change="onCheckboxChange(i)"
                        @delete="onDeleteTodo(i)" />
                </template>
            </ol>
            <p class="empty-list-message" v-html="emptyListMessage"></p>
        </article>

        <AppFooter
            :message="message"
            @filter-change="onFilterChange"
            @clear-completed="onClearCompleted" />
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
            cursor: inherit;

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

	article {
        background-color: var(--todo-background);
        border-top-left-radius: var(--rounded-corners);
        border-top-right-radius: var(--rounded-corners);
        position: relative;
        z-index: 1;
	}

	ol {
		list-style: none;
		padding: 0;
		margin: 0;

        &:empty + .empty-list-message {
            display: block;
        }
	}

    .empty-list-message {
        display: none;
        margin: 0;
        padding: calc(2 * var(--spacing-block)) var(--spacing-inline);
        font-style: italic;
        text-align: center;
        color: var(--text-placeholder);
		border-bottom: 1px solid var(--checkbox);
        font-size: var(--fs-small);
    }
</style>
