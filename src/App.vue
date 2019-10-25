<template>
    <div class="container">
        <div class="row">
            <h1>CSS Animations</h1>
                <hr>
                <select v-model="alertAnimation" class="form-control">
                    <option value="fade">Fade</option>
                    <option value="slide">Slide</option>
                </select>
                <hr>
                <button class="btn btn-primary" @click="show = !show">Show Alert</button>
                <br><br>

            <div class="col-xs-12 col-sm-6">
                <transition name="fade">
                    <div class="alert alert-info" v-if="show">This is some info</div>
                </transition>  
                <transition name="slide" type="animation">
                    <div class="alert alert-info" v-if="show">This is some info</div>
                </transition>
                <transition name="fade" appear>
                    <div class="alert alert-info" v-if="showStart">Animates on page load</div>
                </transition> 
                <transition 
                    enter-active-class="animated bounce"
                    leave-active-class="animated shake"
                >
                    <div class="alert alert-info" v-if="show">Animates on page load and using Animate.css</div>
                </transition>
            </div>
            <div class="col-xs-12 col-sm-6">


                <transition :name="alertAnimation">
                    <div class="alert alert-info" v-if="show">Drop Down: selected animation</div>
                </transition>  
                <transition :name="alertAnimation" type="animation">
                    <div class="alert alert-info" v-if="show">Drop Down: selected animation</div>
                </transition>
                <transition :name="alertAnimation" appear>
                    <div class="alert alert-info" v-if="showStart">Drop Down: selected animation, ANIMATES ON PAGE LOAD</div>
                </transition> 
                <transition 
                    enter-active-class="animated bounce"
                    leave-active-class="animated shake"
                >
                    <div class="alert alert-info" v-if="show">Animates on page load and using Animate.css</div>
                </transition>
            </div>
        </div>
        <div class="row">
            <div class="col-xs-12 col-sm-12">
                <hr>
                <h2>Switching between 2 different contents</h2>
                <transition :name="alertAnimation" mode="out-in">
                    <div class="alert alert-info" v-if="show" key="info">Information</div>
                    <div class="alert alert-warning" v-else key="warning">Warning</div>
                </transition>
            </div>
        </div>
        <div class="row">
            <div class="col">
                <h1>JavaScript Animations</h1>
                <button class="btn btn-primary" @click="load = !load">Load / Remove Element</button>
                <br><br>
                <transition
                @before-enter="beforeEnter"
                @enter="enter"
                @after-enter="afterEnter"
                @enter-canelled="enterCancelled"
                
                @before-leave="beforeLeave"
                @leave="leave"
                @after-leave="afterLeave"
                @leave-cancelled="leaveCancelled"
                :css="false"
                >
                    <div style="width: 300px; height: 70px; background-color: lightgreen;" v-if="load"></div>
                </transition>
                <hr>
                <button class="btn btn-primary"
                    @click="selectedComponent == 'app-success-alert' ? selectedComponent = 'app-danger-alert' : selectedComponent = 'app-success-alert'">Toggle Components</button>
                <br><br>
                <h1>Dynamic Components</h1>
                <!-- use this to switch the car cards from one color to another -->
                <transition name="fade" mode="out-in">
                    <component :is="selectedComponent"></component>
                </transition>              
                <hr>
                <br>
                <h1>Group Transitions</h1>
                <button class="btn btn-primary" @click="addItem">Add Item</button>
                <br><br>
                <ul class="list-group">
                    <transition-group name="slide">
                        <li 
                            class="list-group-item" 
                            v-for="(number, index) in numbers" 
                            @click="removeItem(index)"
                            style="cursor: pointer;"
                            :key="number">{{ number }}</li>
                            <!-- (number, index) retrieve the current index -->
                            <!-- removeItem(index) pass the current index of the component -->
                    </transition-group>
                </ul> 
            </div>
        </div>
    </div>
</template>

<script>
import DangerAlert from './DangerAlert.vue'
import SuccessAlert from './SuccessAlert.vue'
    export default {
        data() {
            return {
                show: false,
                showStart: true,
                alertAnimation: 'fade',
                load: true,
                elementWidth: 100,
                selectedComponent: 'app-success-alert',
                numbers: [1, 2, 3, 4, 5]
            }
        },
        components: {
            appDangerAlert: DangerAlert,
            appSuccessAlert: SuccessAlert
        },
        methods: {
            beforeEnter(el) {
                console.log('beforeEnter')
                this.elementWidth = 100
                el.style.width = this.elementWidth + 'px'
            },
            enter(el, done) {
                console.log('enter')
                let round = 1
                const interval = setInterval(() => {
                    el.style.width = (this.elementWidth + round * 10) + 'px'
                    round++
                    if (round > 20) {
                        clearInterval(interval)
                        done()
                    }
                }, 20)
            },
            // Javascript based animation
            afterEnter(el) {
                console.log('afterEnter')
            },
            enterCancelled(el) {
                console.log('enterCancelled')
            },
            beforeLeave(el) {
                console.log('beforeLeave')
                this.elementWidth = 300
                el.style.width = this.elementWidth + 'px'
            },
            leave(el, done) {
                console.log('leave')
                let round = 1
                const interval = setInterval(() => {
                    el.style.width = (this.elementWidth - round * 10) + 'px'
                    round++
                    if (round > 20) {
                        clearInterval(interval)
                        done()
                    }
                }, 20)
            },
            afterLeave(el) {
                console.log('afterLeave')
            },
            leaveCancelled(el) {
                console.log('leaveCancelled')
            },
            // end JavaScript animation
            // start group animation
            addItem() {
                // randomly puts a new value into the array
                const pos = Math.floor(Math.random() * this.numbers.length)
                this.numbers.splice(pos, 0, this.numbers.length + 1)
            },
            removeItem(index) {
                this.numbers.splice(index, 1)
            }
            // end group animation
        }
    }
</script>

<style>
    .fade-enter {
        opacity: 0;
    }

    .fade-enter-active {
        transition: opacity 1s;
    }

    .fade-leave {
        /* opacity: 1; */
    }

    .fade-leave-active {
        transition: opacity 1s;
        opacity: 0;
    }

    .slide-enter {
        opacity: 0;
    }

    .slide-enter-active {
        animation: slide-in  1s ease-out forwards;
        transition: opacity .5s;
    }

    .slide-leave {

    }

    .slide-leave-active {
        animation: slide-out 1s ease-out forwards;
        transition: opacity 1s;
        opacity: 0;
        position: absolute;
    }

    .slide-move {
        transition: transform 1s;
    }

    .list-group {
        padding-bottom: 300px;
    }

    @keyframes slide-in {
        from {
            transform: translateY(20px);
        }
        to {
            transform: translateY(0);
        }
    }

    @keyframes slide-out {
        from {
            transform: translateY(0);
        }
        to {
            transform: translateY(20px);
        }
    }
</style>
