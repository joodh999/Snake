<script lang="ts">
    export let value;
    let style: string;
    $: style = value === 1 ? "border h-6 bg-black border-black" : "border h-6";
</script>

<div class={style} />
