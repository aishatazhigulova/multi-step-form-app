<template>
    <button
        :class="buttonClass"
        class="btn"
        v-bind="$attrs"
        v-on="$listeners">
        <slot></slot>
    </button>
</template>

<script>
export default {
    inheritAttrs: false,
    props: {
        buttonClass: {
            type: String
        }
    }
}
</script>

<style>
button, input[type="submit"], input[type="reset"] {
    background: none;
    color: inherit;
    border: none;
    padding: 0;
    font: inherit;
    cursor: pointer;
    outline: inherit;
}

button:disabled,
button[disabled] {
    background-color: var(--color-primary-grey);
    border: 1px solid var(--color-primary-grey);
    cursor: not-allowed;
}

.btn {
    font-size: 1rem;
    border: 1px solid var(--color-primary);
    background-color: var(--color-primary);
    border-radius: var(--br-small);
    padding: 0.4rem;
    width: 10rem;
    height: 2.8rem;
}

.btn-wide {
    width: 100%;
}

.btn-left {
    margin-left: auto;
}


</style>
