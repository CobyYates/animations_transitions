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
                >
                    <div style="width: 100px; height: 100px; background-color: lightgreen;" v-if="load"></div>
                </transition>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                show: false,
                showStart: true,
                alertAnimation: 'fade',
                load: true,
            }
        },
        methods: {
            beforeEnter(el) {
                console.log('beforeEnter')
            },
            enter(el, done) {
                console.log('enter')
                done()
            },
            afterEnter(el) {
                console.log('afterEnter')
            },
            enterCancelled(el) {
                console.log('enterCancelled')
            },
            beforeLeave(el) {
                console.log('beforeLeave')
            },
            leave(el, done) {
                console.log('leave')
                done()
            },
            afterLeave(el) {
                console.log('afterLeave')
            },
            leaveCancelled(el) {
                console.log('leaveCancelled')
            }
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
