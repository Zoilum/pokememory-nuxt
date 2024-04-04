<script setup lang="ts">
interface Props {
  status: 'idle' | 'pending' | 'success' | 'error'
}

const {status} = defineProps<Props>()
</script>

<template>
    <div class="spinner-wrapper">
        <h1 v-if="status === 'error'">Failed to catch pokemons!</h1>
        <h1 v-else-if="status === 'pending'">Trying to catch pokemons...</h1>
        <svg viewBox="0 0 100 100" width="150" height="150" :class='status'>
            <g transform="translate(50 50) scale(0.8)">
                <g class="stars" transform="scale(0)">
                    <path fill="#303030" stroke="#303030" stroke-width="3" stroke-linejoin="round" stroke-linecap="round"
                        d="M -50 -50 l 4.5 0 l 2.5 -4.5 l 2.5 4.5 l 4.5 0 l -3.5 3.5 l 1.5 5 l -5 -2.5 l -5 2.5 l 1.5 -5 l -3.5 -3.5">
                    </path>
                    <path fill="#303030" stroke="#303030" stroke-width="3" stroke-linejoin="round" stroke-linecap="round"
                        d="M 36 -50 l 4.5 0 l 2.5 -4.5 l 2.5 4.5 l 4.5 0 l -3.5 3.5 l 1.5 5 l -5 -2.5 l -5 2.5 l 1.5 -5 l -3.5 -3.5">
                    </path>
                </g>
                <g transform="translate(0 50)">
                    <g class="gravity">
                        <g transform="translate(0 -50)">
                            <g class="ball" transform="scale(1 1)">
                                <g class="bottom">
                                    <path fill="#ffffff" stroke="#303030" stroke-width="5"
                                        d="M -47.5 0 a 47.5 47.5 0 0 0 95 0z"></path>
                                </g>
                                <g class="top">
                                    <path fill="#f15d5f" d="M -47.5 0 a 47.5 47.5 0 0 1 95 0"></path>
                                    <path fill="none" stroke="#ffffff" stroke-width="5" stroke-linecap="round"
                                        stroke-dasharray="0 15 9 9 20 100" d="M -38 -0 a 38 38 0 0 1 76 0"></path>
                                    <path fill="none" stroke="#303030" stroke-width="5"
                                        d="M -47.5 0 a 47.5 47.5 0 0 1 95 0z"></path>
                                </g>

                                <g class="open" transform="scale(1 0)">
                                    <path fill="#303030" stroke="#303030" stroke-width="5" stroke-linejoin="round"
                                        d="M -47.5 -10 a 190 190 0 0 1 95 0 a 190 190 0 0 1 -95 0"></path>
                                    <path fill="#303030" stroke="#303030" stroke-width="5" stroke-linejoin="round"
                                        d="M -47.5 5 a 160 160 0 0 0 95 0 a 180 180 0 0 0 -95 0"></path>
                                </g>

                                <g class="center">
                                    <circle fill="#ffffff" stroke="#303030" stroke-width="5" cx="0" cy="0" r="12">
                                    </circle>
                                    <circle fill="#ffffff" stroke="#303030" stroke-width="3" cx="0" cy="0" r="6">
                                    </circle>
                                    <g class="inner" opacity="0">
                                        <circle fill="#f15d5f" cx="0" cy="0" r="4.5"></circle>
                                    </g>
                                </g>
                            </g>
                        </g>
                    </g>
                </g>
            </g>
        </svg>
    </div>
</template>

<style>
.spinner-wrapper {
    background: rgb(25, 27, 29);
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    color: #eee;
}

/* POKEBALL ANIMATIONS */
/* shake to rotate g.gravity */
svg.pending g.gravity {
    /* ! infinite given the unknown time it takes for the request to be resolved/rejected */
    animation: shake 0.75s infinite cubic-bezier(0.645, 0.045, 0.355, 1);
}

@keyframes shake {
    20% {
        transform: rotate(-10deg);
    }

    60% {
        transform: rotate(10deg);
    }

    80% {
        transform: rotate(0deg);
    }
}

/* pulse to change the color of the innermost circle */
svg.pending g.inner {
    /* ! infinite given the unknown time it takes for the request to be resolved/rejected */
    animation: pulse 0.75s infinite cubic-bezier(0.645, 0.045, 0.355, 1);
}

@keyframes pulse {
    25% {
        opacity: 0.7;
    }

    50% {
        opacity: 0;
    }
}

/* squash for the pokeball and the success animation */
svg.success g.ball {
    animation: squash 0.2s cubic-bezier(0.645, 0.045, 0.355, 1) 2 alternate;
}

@keyframes squash {
    to {
        transform: scaleY(0.9);
    }
}

/* scaleUp for the stars and the success animation */
svg.success g.stars {
    animation: scaleUp 0.2s 0.2s cubic-bezier(0.645, 0.045, 0.355, 1) forwards;
}

@keyframes scaleUp {
    to {
        transform: scale(1);
    }
}

/* scaleUp for the arcs in between the ball's halves and for the error animation */
svg.error g.open {
    animation: scaleUp 0.2s 0.1s cubic-bezier(0.645, 0.045, 0.355, 1) forwards;
}

/* translateUp for the top half and the error animation */
svg.error g.top {
    animation: translateUp 0.3s cubic-bezier(0.645, 0.045, 0.355, 1) forwards;
}

@keyframes translateUp {
    to {
        transform: translateY(-10px);
    }
}

/* translateDown for the bottom half and the error animation */
svg.error g.bottom {
    animation: translateDown 0.3s cubic-bezier(0.645, 0.045, 0.355, 1) forwards;
}

@keyframes translateDown {
    to {
        transform: translateY(5px);
    }
}

/* translateHigher for the center and the error animation */
svg.error g.center {
    animation: translateHigher 0.3s cubic-bezier(0.645, 0.045, 0.355, 1) forwards;
}

@keyframes translateHigher {
    to {
        transform: translateY(-18px);
    }
}

.spinner {
    background: rgb(25, 27, 29);
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    color: #eee;
}
</style>