<style lang="scss">
    @import url("https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css");
    @import url('https://fonts.googleapis.com/css2?family=Open+Sans:ital,wght@0,300..800;1,300..800&display=swap');

    $font-monospace: SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;
    $font-normal: 'Open Sans', Arial, Helvetica, Verdana, sans-serif;

    main {
        font-family: $font-normal;
        display: flex;
        flex-direction: column;
        width: 100%;
        height: 100%;
        #frame {
            border: 0;
            display: flex;
            flex: 1;
        }
        .controls {
            position: absolute;
            background: #fff;
            color: black;
            padding: .5em;
            margin: 1em;
            border-radius: .5em;
            font-size: 1.5rem;
            display: flex;
            gap: 5px;
            .current {
                font-family: $font-monospace;
                display: flex;
                margin-right: 10px;
                justify-content: center;
                align-items: center;
            }
            button {
                font-size: inherit;
                display: flex;
                justify-content: center;
                align-items: center;
            }
        }
    }
</style>

<main>
    <div class="controls">
        <b class="current">{frameNum}</b>
        <button><i class="bi bi-caret-left{leftFilled ? '-fill' : ''}"></i></button>
        <button><i class="bi bi-caret-right{rightFilled ? '-fill' : ''}"></i></button>
    </div>
    <iframe id="frame" src="https://kes.net/events.aspx?positionId={frameNum.toString()}" title="KES Events" frameborder="0"></iframe>
</main>

<script lang="ts">
    import { onMount } from 'svelte';

    let frameNum = 1000;
    let leftFilled = true;
    let rightFilled = true;

    const checkEnd = () => {
        leftFilled = (frameNum <= 0) ? false : true;
        rightFilled = (frameNum >= 1000) ? false : true;
    }

    const findNextPage = num => {
        while (res) {
            frameNum = (num == -1) ? frameNum - 1 : frameNum + 1;
            let res = checkPageEmpty(`https://kes.net/events.aspx?positionId=${frameNum}`);
        }
    }

    checkEnd();

    onMount(() => {
        const checkPageEmpty = url => {
            fetch(url)
                .then(response => response.text())
                .then(text => {
                    const parser = new DOMParser();
                    const doc = parser.parseFromString(text, "text/html");
                    return doc;
                })
        };
    });
</script>