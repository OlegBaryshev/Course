<template>
  <section class="terminal-controller">
    <ide-header :isFullscreen="this.isFullscreen" :title="'Терминал'" @fullScreen="$event=>isFullscreen=$event.status"></ide-header>
    <div class="layout-terminal">
        <article class="controller-terminal-result">
            <div class="controller-line"></div>
        </article>
        <form @submit.prevent class="controller-input-form">
            <textarea class="controller-input" v-model="this.terminalText"></textarea>
        </form>
    </div>
  </section>
</template>

<script>
import ideHeader from '@/views/practice/ide/ide-header.vue'
export default {
    components:{
        ideHeader
    },
    props:{
        ideText:{
            default:'',
            required:true,
            type:String
        }
    },
    data(){
        return{
            isFullscreen:false,
            terminalText:'',
            data_ideText:this.ideText,
        }
    },
    watch:{
        'ideText': {
            immediate: true, 
            handler(val, oldVal) {
                this.data_ideText = this.ideText;
                this.getSplit()
            },
            deep: true,
        }
    },
    methods:{
        getSplit(){
            let messa = "";
            (function () {
                var old = console.log;
                console.log = function (message) {
                    if (typeof message == 'object') {
                        return(JSON && JSON.stringify ? JSON.stringify(message) : message);
                    } else {
                        return message;
                    }
                }
            })();
            try{
                messa = (String(eval(this.data_ideText)))
            }catch(e){
                this.terminalText = String(e)
            }
            this.terminalText = messa;
            console.log(this.terminalText)
        }
    }
}
</script>

<style lang="scss" scoped>
    .terminal-controller{
        box-shadow:0 0 0.2vmax rgba(0, 0, 0, 0.157);
        height: 100%;
        .layout-terminal{
            width: 100%;
            height: 100%;
            .controller-input-form{
                --standard-form-border:0.1vmax;
                --form-padding:1vmax;
                width: 100%;
                padding: 0 var(--form-padding);
                padding-bottom: var(--form-padding);
                display: flex;
                justify-content: center;
                align-items: center;
                .controller-input{
                        display: flex;
                        min-width: 100%;
                        padding: 0vmax 1vmax;
                        font-size: 1vmax;
                        line-height: 1em;
                        font-stretch:1em;
                        white-space:nowrap;
                        resize: none;
                        overflow: hidden;
                }
            }
        }
    }
</style>