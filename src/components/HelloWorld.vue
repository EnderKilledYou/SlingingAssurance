<template>
  <div>

    {{ Text }}
<button @click="Start">Start</button>
    <button @click="Stop">Stop</button>
  </div>
</template>

<script lang="ts">
import {Component, Prop, Vue} from 'vue-property-decorator';

@Component
export default class HelloWorld extends Vue {
  Text: string[] = []
  Recognition?: any
  @Prop() private msg!: string;

  async Start() {
    this.Recognition.start();
  }
  async Stop() {

    this.Recognition.stop();
  }
  mounted() {
    //@ts-ignore
    const SpeechRecognition = window.SpeechRecognition ? window.SpeechRecognition : window.webkitSpeechRecognition;
    if (!SpeechRecognition) {
      throw new Error(
          "Speech Recognition does not exist on this browser. Use Chrome or Firefox"
      );
    }
    this.Recognition = new SpeechRecognition();
    this.Recognition.interimResults = true
    this.Recognition.addEventListener("result", this.OnSpeechResult.bind(this))
    this.Recognition.addEventListener("end", this.End.bind((this)));
    this.Recognition.onresult = this.OnSpeechResult.bind(this)
  }

  async End(e: any) {
    
  }



  private OnSpeechResult(e: any) {
    console.log(e);
debugger;
    this.Text = Array.from(e.results).map((r: any) => r[0]).map(r => r.transcript);
  }
}
</script>
