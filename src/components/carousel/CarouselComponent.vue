<template>
    <div class="carousel">
        <slot></slot>
        <button class="carousel__nav carousel__next" @click.prevent="next">Next</button>
        <button class="carousel__nav carousel__prev" @click.prevent="previous">Previous</button>
        <div class="carousel__pagination">
            <button v-for="slide in slides" :key="slide.index" :class="{active: slide.index === index}"
                @click="goTo(slide.index)">{{slide.index}}</button>
        </div>
    </div>
</template>

<script>
export default {
    data() {

        return {
            index: 0,
            slides: [],
            direction: 'right'
        } 
    },
    watch: {
        slides () {
            if(this.index >= this.slidesCount){
                this.index = this.slidesCount - 1
            }
        }
    },
    computed: {
        slidesCount() { return this.slides.length }
    },
    methods: {
        next() {
            this.index++;
            this.direction = 'right';
            if(this.index >= this.slidesCount ) this.index = 0;
        },
        previous() {
            this.index--;
            this.direction = 'left';
            if(this.index < 0) this.index = this.slidesCount-1;
        },
        goTo(slideIndex) {
            this.direction = (slideIndex > this.index) ? 'right' : 'left';

            if(slideIndex >= 0 && slideIndex < this.slidesCount) this.index = slideIndex;
        }
    },
    mounted() {
        this.slides = this.$children;
    },
}
</script>

<style>
    .carousel {
        position: relative; 
        overflow: hidden;  
    }

    .carousel__nav {
        position: absolute;
        top: 50%;
        margin-top: -31px;
        left: 10px;
        background-color: rgba(0, 0, 0, 0.467);
        color: #fff;
        font-weight: 600;
        border-radius: 100px;
        font-size: 1.1em;
        padding: 0 20px;
        min-width: 103px;
        height: 63px;
    }

    .carousel__nav:hover, .carousel__nav.carousel__next:hover {
        background-color: rgb(0, 0, 0);
        cursor: pointer;
    }
    
    .carousel__nav.carousel__next {
        right: 10px;
        left: auto;
        background-color: rgba(0, 0, 0, 0.467);
    }

    .carousel__pagination {
        position: absolute;
        bottom: 10px;
        left: 0;
        right: 0;
        text-align: center;
    }

    .carousel__pagination button{
        display: inline-block;
        width: 20px;
        height: 20px;
        background-color: #000;
        opacity: .8;
        border-radius: 10px;
        margin: 0 2px;
    }

    .carousel__pagination button.active{
        background-color: #fff;
    }
</style>