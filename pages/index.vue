<!-- <script setup>

// layout
definePageMeta({
  layout: "default",
});

defineProps({
  modelValue : {
      type :  null,
      required :  true
  }
})

defineEmits({
  update:modelValue
})



// import
import { ref } from 'vue';

// define

// data
const ctx = ref(null)
const sign = ref(false)
const prevX = ref(0)
const prevY = ref(0)

// methods
onMounted(() => {
  ctx.value  = $el.getContext('2d')
  ctx.value.strokeStyle  =  'black'
  ctx.value.lineWidth  =  2
})

const onMouseDown = ($event) => {
  sign.value = true
  prevX.value = $event.offsetX
  prevY.value = $event.offsetY
}

const onMouseMove = ($event) => {
  if(this.sign) {
      const  currX  = $event.offsetX
      const  currY  = $event.offsetY
      this.draw(prevX.value, prevY.value, currX, currY)
      .value  =  currX
      prevY  =  currY
  }
}

const draw = (depX, depY, destX, destY) => {
  ctx.beginPath()
    ctx.moveTo(depX, depY)
    ctx.lineTo(destX, destY)
    ctx.closePath()
    ctx.stroke()

    const img = $el.toDataURL('image/png').replace('image/png',        'image/octet-stream')
    $emit('update:modelValue', img)
}

// watch
watch(()=>(modelValue), (model)=>{
  if(!model) {
    ctx.clearRect(0, 0, $el.width, $el.height)
  }
})


</script> -->

<script>

export  default {
    emits : ['update:modelValue'],
    props : {
        modelValue : {
            type :  null,
            required :  true
        }
    },
    data() {
        return {
            ctx :  null,
            sign :  false,
            prevX :  0,
            prevY :  0,
        }
    },
    methods : {
        mousedown($event) {
            this.sign  =  true
            this.prevX  = $event.offsetX
            this.prevY  = $event.offsetY
        },
        mousemove($event) {
            if(this.sign) {
                const  currX  = $event.offsetX
                const  currY  = $event.offsetY
                this.draw(this.prevX, this.prevY, currX, currY)
                this.prevX  =  currX
                this.prevY  =  currY
            }
        },
        draw(depX, depY, destX, destY) {
            this.ctx.beginPath()
            this.ctx.moveTo(depX, depY)
            this.ctx.lineTo(destX, destY)
            this.ctx.closePath()
            this.ctx.stroke()

            const img = this.$el.toDataURL('image/png').replace('image/png', 'image/octet-stream')
            this.$emit('update:modelValue', img)
        },
    },
    watch : {
        modelValue(model) {
            if(!model) {
            this.ctx.clearRect(0, 0, this.$el.width, this.$el.height)
            }
        }
    },
    mounted() {
        this.ctx  = this.$el.getContext('2d')
        this.ctx.strokeStyle  =  'black'
        this.ctx.lineWidth  =  2
    }
}


</script>


<template>
  <div>
    <h1 class="">Welcome to the homepage</h1>
    <AppAlert>
      This is an auto-imported component
    </AppAlert>

    <canvas @mousedown="onMouseDown" 
            @mousemove="onMouseMove"
            @mouseup="sign = false" 
            @mouseout="sign = false" />
  </div>
</template>

<style scoped >
  canvas {
    border: 1px solid black;
    background-color: white;
    cursor: crosshair;
  }
</style>