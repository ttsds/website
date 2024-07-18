<script>
    export let title;
    export let content;
</script>

<div class="cell card">
    <div class="card-content">
        <div class="content">
            <h2>{@html title}</h2>
            {@html content}
        </div>
    </div>
</div>
