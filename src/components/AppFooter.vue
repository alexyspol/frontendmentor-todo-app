<script setup>
    defineProps(['message']);
</script>

<template>
    <footer>
        <div class="section section__message">
            <span>{{ message }}</span>
        </div>
        <div class="section section__filters">
            <label>
                <input type="radio" name="options" checked @change="$emit('filterChange', 'all')">
                <span>All</span>
            </label>
            <label>
                <input type="radio" name="options" @change="$emit('filterChange', 'active')">
                <span>Active</span>
            </label>
            <label>
                <input type="radio" name="options" @change="$emit('filterChange', 'completed')">
                <span>Completed</span>
            </label>
        </div>
        <div class="section section__clear-completed">
            <button @click="$emit('clearCompleted')">Clear Completed</button>
        </div>
    </footer>
</template>

<style scoped>
    footer {
        display: grid;
        grid-template-areas:
            "message clear"
            "filters filters";
        font-size: var(--fs-small);
        color: var(--footer-text);
        filter: drop-shadow(0 1rem 4rem rgba(0, 0, 0, .2));
    }

    .section {
		padding: var(--spacing-block) var(--spacing-inline);
        background-color: var(--todo-background);
    }

    .section__message {
        grid-area: message;
        border-bottom-left-radius: var(--rounded-corners);
    }

    .section__filters {
        grid-area: filters;
        margin-top: var(--spacing-block);
        border-radius: var(--rounded-corners);
        text-align: center;
    }

    .section__clear-completed {
        grid-area: clear;
        border-bottom-right-radius: var(--rounded-corners);
        text-align: right;
    }

    /*  The 62.5% trick doesn't affect media queries.
        In media queries, 1rem remains 16px, not 10px.
        Therefore, 600px is 37.5rem, not 60rem. */
    @media (min-width: 37.5rem) {
        footer {
            grid-template-areas: "message filters clear";
        }

        .section__filters {
            margin: 0;
            border-radius: 0;
        }
    }

    label {
        cursor: pointer;
        font-weight: 700;

        &:not(:last-child) {
            margin-right: var(--spacing-inline);
        }
    }

    input {
        display: none;
    }

    :checked + span,
    button:active {
        color: var(--primary);
    }

    button {
		font-family: inherit;
        padding: 0;
        border: none;
        background: none;
        font-size: inherit;
        cursor: pointer;
        color: inherit;
    }

    label:hover,
    button:hover {
        color: var(--footer-text-hover);
    }
</style>
