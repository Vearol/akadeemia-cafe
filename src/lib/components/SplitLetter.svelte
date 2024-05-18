<script>
    export let word = 'test';
    export let className = '';
</script>
{#each word.split('') as letter}
    <span class={className}>{letter}</span>
{/each}