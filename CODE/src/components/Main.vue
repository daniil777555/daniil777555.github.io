<template>
    <div class="container" :class="{'gray-container': isOpen}">
        <button v-on:click="isOpen = !isOpen" class="btn-tax">Налоговый вычет</button>
        <TXD v-show="isOpen" v-on:isOpen="isOpen = $event"/>
    </div>
</template>

<script>
import TXD from './TaxDeduction.vue'

export default {
    components: {
        TXD
    },

    data(){
        return{
            isOpen: false
        }
    },

    methods:{

    }
}
</script>

<style scope>

    .container{
        height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
        background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56;
        box-shadow: 0px -0.11px 16.9495px rgba(183, 187, 225, 0.33);
        border-radius: 0px;
    }

    .gray-container{
        background: rgba(0, 0, 0, 0.3);
    }
    
    .btn-tax{
        cursor: pointer;
        border: 1px solid #FFFFFF;
        box-sizing: border-box;
        filter: drop-shadow(0px 0px 44px #CFDAE7);
        border-radius: 6px;
        font-weight: 400;
        background: none;
        color: #fff;
        line-height: 24px;
        padding: 16px 32px;
        font-size: 16px;
    }

    .btn-tax:hover{
        background: #fff;
        color: #000;
        filter: none;
    }

    @media screen and (max-width: 320px) {
        .btn-tax{
            font-size: 12px;
            line-height: 16px;
            padding: 12px 24px;
        }
    }

</style>