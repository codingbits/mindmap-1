<template>
    <g class="line">
        <path :id="pathId" :d="currentPathDef">
        </path>
    </g>
</template>

<script>
    import * as d3 from "d3"
    export default {
        data(){
            return {
                id: "",
                x1: 0,
                y1: 0,
                x2: 0,
                y2: 0,

                c_x1: 0,
                c_y1: 0,
                c_x2: 0,
                c_y2: 0,

                parentNode: null,
                childNode: null,

                animating: false,

                animatable: true,
                animateAttr: ["x1", "y1", "x2", "y2"]
            }
        },
        computed: {
            pathDef(){
                return `M${this.x1},${this.y1} L${this.x2},${this.y2}`
            },
            currentPathDef(){
                return `M${this.c_x1},${this.c_y1} L${this.c_x2},${this.c_y2}`
            },
            pathId(){
                return `${this.id}-path`
            }
        },
        beforeMount(){
            // Layout update must happen before $mount
            this.animateAttr.map((attr)=>{
                this["c_"+attr] = this[attr]
            })
        },
        methods: {
            async animate(){   
                this.animating = true
                let path = d3.select(`#${this.pathId}`)
                await new Promise((resolve)=>path.transition().attr("d", this.pathDef).on('end', resolve))
                this.animating = false
            },
            async layoutUpdated(){
                if(this.$el && this.animatable){
                    try{
                        await this.animate()
                    } catch(err){
                        console.log(err)
                    }
                }
                if(!this.animating){
                    this.animateAttr.map((attr)=>{
                        this["c_"+attr] = this[attr]
                    })
                }
            }
        },
        beforeDestroy(){
            this.$parent.$el.removeChild(this.$el)
        }
    }

    async function delay(ms){
        return new Promise((resolve)=>setTimeout(resolve, ms))
    }

// startAnimate: (function(){
//     let frames = 30
//     let startTime
//     let duration = 300
//     let x1, y1, x2, y2 // Target Location
//     let timer // Interval Timer
//     return function(){
//         // When called, update target location, startTime

//         let nextFrame = ()=>{
//             // Access current location
//             let c_x1, c_y1, c_x2, c_y2 // Current Location
//             // Calculate next frame
//             let n_x1, n_y1, n_x2, n_y2 // Current Location
//             // If current location is target position, return, invalidate timer

//             // Else, Update current location

//             console.log("Animateing")
//         }
        
//         timer = setInterval(nextFrame, 1000/frames)
//     }
// })(),

</script>


<style lang="scss">
    .line {
        stroke: black;
        stroke-width: 1;

        line {
            transition: all 0.3s ease-in-out;
        }
    }
</style>