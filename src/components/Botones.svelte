<script>
    export let years;
    export let currentIndex;
    export let displayYear;
    export let isPlaying;

    export let onToggle;
    export let onRestart;
    export let onJumpToIndex;
    export let onStepBack;
    export let onStepForward;
    export let onScrubberInput;
    export let onScrubberChange;

    // Sincronización reactiva para que la bola no tenga "lag"
    $: currentPos = years.indexOf(displayYear);
</script>

<div class="timeline-panel">
    <div class="action-btns">
        <button class="btn-play" on:click={onToggle} class:active={isPlaying}>
            {#if isPlaying}
                <svg
                    width="14"
                    height="14"
                    viewBox="0 0 24 24"
                    fill="currentColor"
                >
                    <rect x="6" y="4" width="4" height="16" rx="1" />
                    <rect x="14" y="4" width="4" height="16" rx="1" />
                </svg>
                <span>PAUSA</span>
            {:else}
                <svg
                    width="14"
                    height="14"
                    viewBox="0 0 24 24"
                    fill="currentColor"
                >
                    <path d="m7 3 14 9-14 9z" />
                </svg>
                <span>COMEÇAR</span>
            {/if}
        </button>

        <button class="btn-restart" on:click={onRestart}>
            <svg
                width="14"
                height="14"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2.5"
                stroke-linecap="round"
                stroke-linejoin="round"
            >
                <path d="M3 12a9 9 0 1 0 9-9 9.75 9.75 0 0 0-6.74 2.74L3 8" />
                <path d="M3 3v5h5" />
            </svg>
            REINÍCIO
        </button>
    </div>

    <div class="timeline-track">
        <button
            class="icon-btn"
            on:click={onStepBack}
            disabled={currentIndex === 0}
            title="Año anterior"
        >
            <svg
                width="12"
                height="12"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="3"
                stroke-linecap="round"
                stroke-linejoin="round"
            >
                <path d="m15 18-6-6 6-6" />
            </svg>
        </button>

        <div class="scrubber-wrap">
            <div class="year-labels">
                {#each years as yr}
                    <button
                        class="year-label"
                        class:active={yr === displayYear}
                        on:click={() => onJumpToIndex(years.indexOf(yr))}
                        >{yr}</button
                    >
                {/each}
            </div>
            <input
                type="range"
                min="0"
                max={years.length - 1}
                step="1"
                value={isPlaying ? currentPos : currentIndex}
                on:input={onScrubberInput}
                on:change={onScrubberChange}
                class="scrubber"
            />
        </div>

        <button
            class="icon-btn"
            on:click={onStepForward}
            disabled={currentIndex === years.length - 1}
            title="Año siguiente"
        >
            <svg
                width="12"
                height="12"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="3"
                stroke-linecap="round"
                stroke-linejoin="round"
            >
                <path d="m9 18 6-6-6-6" />
            </svg>
        </button>
    </div>
</div>

<style>
    .timeline-panel {
        display: flex;
        flex-direction: column;
        padding: 0px 0px 20px 0px;
        border-bottom: 1px solid grey;
        margin-top: 10px;
        margin-bottom: 20px;
        user-select: none;
    }

    .action-btns {
        display: flex;
        gap: 12px;
        margin-bottom: 20px;
    }

    .timeline-track {
        display: flex;
        align-items: center;
        gap: 10px;
        /* Aseguramos que sea visible por defecto */
        visibility: visible;
        height: auto;
    }

    /* --- ESTO ES LO QUE OCULTA EL CRONOGRAMA EN MÓVIL --- */
    @media (max-width: 675px) {
        .timeline-track {
            display: none !important;
        }

        .timeline-panel {
            padding-bottom: 0px;
            margin-bottom: 15px;
            border-bottom: none; /* Opcional: quitamos la línea en móvil para ganar espacio */
        }

        .action-btns {
            margin-bottom: 0px;
        }
    }

    /* ... El resto de estilos (scrubber, labels, etc.) se mantienen igual ... */

    .scrubber-wrap {
        flex: 1;
        display: flex;
        flex-direction: column;
        gap: 5px;
        padding: 0 4px;
    }

    .year-labels {
        display: flex;
        justify-content: space-between;
        margin: 0 -10px;
    }

    .year-label {
        background: none;
        border: none;
        padding: 0;
        font-size: 0.65rem;
        font-weight: 400;
        color: #94a3b8;
        cursor: pointer;
        font-variant-numeric: tabular-nums;
        width: 32px;
        text-align: center;
    }

    .year-label.active {
        color: #212c55;
        font-weight: 800;
    }

    .scrubber {
        width: 100%;
        height: 3px;
        -webkit-appearance: none;
        background: #e2e8f0;
        border-radius: 10px;
        outline: none;
        cursor: pointer;
    }

    .scrubber::-webkit-slider-thumb {
        -webkit-appearance: none;
        width: 10px;
        height: 10px;
        background: #212c55;
        border-radius: 50%;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        cursor: grab;
    }

    .icon-btn {
        background: white;
        border: 1px solid #e2e8f0;
        border-radius: 8px;
        width: 24px;
        height: 24px;
        display: flex;
        align-items: center;
        justify-content: center;
        color: #64748b;
        cursor: pointer;
    }

    .btn-play,
    .btn-restart {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 5px;
        padding: 8px 15px;
        border-radius: 10px;
        font-weight: 700;
        font-size: 0.75rem;
        cursor: pointer;
        border: none;
    }

    .btn-play {
        background: #212c55;
        color: white;
    }
    .btn-play:hover {
        background: #0f1326;
    }
    .btn-play.active {
        background: #cc3d3d;
    }
    .btn-play.active:hover {
        background: #a63232;
    }
    .btn-restart {
        background: white;
        color: #475569;
        border: 1px solid #e2e8f0;
    }
    .btn-restart:hover {
        background: #f2f2f2;
    }
</style>
