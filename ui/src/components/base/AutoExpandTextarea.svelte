<script>
    export let value = "";
    export let maxHeight = 200;

    let inputElem;
    let updateTimeoutId;

    $: if (inputElem && typeof value !== undefined) {
        updateInputHeight();
    }

    function updateInputHeight() {
        clearTimeout(updateTimeoutId);
        updateTimeoutId = setTimeout(() => {
            if (inputElem) {
                inputElem.style.height = ""; // reset
                inputElem.style.height = Math.min(inputElem.scrollHeight + 2, maxHeight) + "px";
            }
        }, 0);
    }

    // Pressing "Enter" key should trigger parent form submission,
    // aka. the same as any <input /> element.
    //
    // note: New line could be added using "Enter+Shift".
    function handleKeydown(e) {
        if (e?.code === "Enter" && !e?.shiftKey) {
            e.preventDefault();

            // trigger parent form submission (if any)
            const form = inputElem.closest("form");
            form?.requestSubmit && form.requestSubmit();
        }
    }
</script>

<textarea bind:this={inputElem} bind:value on:keydown={handleKeydown} {...$$restProps} />

<style>
    textarea {
        resize: none;
        padding-top: 4px !important;
        padding-bottom: 5px !important;
        min-height: var(--inputHeight);
        height: var(--inputHeight);
    }
</style>
