<template>
    <div id="overlay">
        <div class="close" @click="toggleCollapse">
            <div id="timerTitle" v-if="collapsed">➖</div>
            <img v-else id="timerImg" src="~@/assets/timer.png" />
        </div>
        <div id="timer" v-if="collapsed">
            <div>{{formatedTimeout}}</div>
            <button @click="startTimer" v-if="timer.pause">Start timer</button>
            <button @click="pauseTimer" v-else>Pause timer</button>
        </div>
    </div>
</template>
<script>
import useCollapse from './useCollapse'
import useTimeFormat from './useTimeFormat'
import { reactive, computed, watch, toRefs } from 'vue'

export default {
    setup(props) {
        const { time } = toRefs(props);
        const timer = reactive({
            timeout: 3,
            pause: true,
            timer: 0
        });
        const startTimer = () => {
            timer.pause = false;
            timer.timer = setInterval(() => {
                if (timer.timeout > 0) {
                timer.timeout--
                } else {
                pauseTimer()
                }
            }, 1000);
        };
        const pauseTimer = () => {
            timer.pause = true;
            clearTimeout(timer.timer);
        };
        watch(time, (newTime) => {
            pauseTimer();
            timer.timeout = newTime;
        });
        const { toHoursString, toMinutesString, toTwoDigitString } = useTimeFormat();
        const formatedTimeout = computed(() => {
            return `${toHoursString(timer.timeout)}:${toMinutesString(timer.timeout)}:${toTwoDigitString(timer.timeout % 60)}`;
        });
        const { collapsed, toggleCollapse } = useCollapse();
    
        return {
            time,
            timer,
            formatedTimeout,
            startTimer,
            pauseTimer,
            collapsed,
            toggleCollapse,
        };
    },
    props: {
        time: Number
    },
};
</script>
<style>
#overlay {
    position: fixed;
    right: 1em;
    top: 60px;
    z-index: 2;
}

#overlay .close {
    right: 1em;
    z-index: 3;
}

#timer {
    text-transform: uppercase;
    position: relative;
    width: 216px;
    height: 60px;
    right: 1em;
    top: 0;
    text-align: center;
    color:#35495e;
    padding: 60px 0;
}

#timerImg {
    width: 40px;
}

#timer div {
    font-size: 2em;
    padding-bottom: 10px;
}

#timer button {
    color: white;
    text-transform: uppercase;
    background-color:#41b883;
    border: #35495e solid 3px;
    border-radius: 2em;
    font: inherit;
    padding: 0.35em 1em;
}

#timer button:focus {
    outline: none;
    background-color: #35495e;
}
</style>