<template>
    <div>
      <timer
        v-if="!complete"
        :time="nextTimer"
      />
      <h1>
          Pizz TODO
      </h1>
      <part
          v-for="(part, index) in parts"
          :key="part.title"
          :title="part.title"
          :description="part.description"
          :complete="part.complete"
          @click="handleClick(index)"
      />
      <p v-if="!complete">{{totalSteps}} from {{parts.length}} is ready</p>
      <p v-else>Done</p>
    </div>
</template>
<script>
import RecipePart from './RecipePart';
import Timer from './Timer';

export default {
  data() {
    return {
      parts: [
        {
          title: 'Prepare for work',
          description: 'Prepare all needed things',
          time: 6
        },
        {
          title: 'Do first step',
          description: 'Wait 10 min. You can use the timer',
          time: 1
        },
        {
          title: 'Do something',
          description: 'You can choose all staff that you can find. Two variants <ol> <li>Get and do;</li> <li>Start to prepare.</li> </ol>',
          time: 2
        },
        {
          title: 'Coocking',
          description: 'Super step',
          time: 3
        },
        {
          title: 'Done',
          description: 'Click the button and wait the guests',
          time: 0
        }
      ],
      nextTimer: 0
    };
  },
  methods: {
    handleClick(index) {
      this.parts[index].complete = !this.parts[index].complete;
      if(index < this.parts.length - 1 && this.parts[index + 1].time) {
          this.nextTimer = this.parts[index].time;
      }
    }
  },
  components: { part: RecipePart, Timer },
  computed: {
    totalSteps() {
      return this.parts.reduce((count, part) => part.complete ? count + 1 : count, 0);
    },
    complete() {
      return this.parts.every(part => part.complete);
    }
  }
}
</script>
<style >
h1, h2, h3, h4, h5, h6 {
  margin: auto;
  font-size: 1.25em;
  font-weight: normal;
}

ol, ul {
  margin: auto;
}

.delimiter h2 {
  margin: auto;
  display: flex;
  width: 100%;
}

.delimiter h2:after {
  content: '';
  display: block;
  position: relative;
  margin-left: 0.5em;
  margin-right: 1em;
  flex: 1;
  height: 1px;
  top: 0.625em;
  border-bottom: 1px solid black;
}

.delimiter p {
  margin: 0.25em 1em;
}

.delimiter {
  cursor: pointer;
  position: relative;
  text-align: left;
  margin: 1em auto;
  max-width: 600px;
}

.delimiter.complete {
  color: #aaa;
}

.right {
  float: right;
}

.close {
  font-size: 1em !important;
  position: absolute;
  cursor: pointer;
  right: 0;
  top: 0;
}

</style>