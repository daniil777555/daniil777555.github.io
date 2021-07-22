<template>
    <div class="txd">
        <p class="close" v-on:click="$emit('isOpen', false)"><svg class="svg-close" xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 0 24 24" width="26px" fill="#EA0029"><path d="M0 0h24v24H0V0z" fill="none"/><path d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12 19 6.41z"/></svg></p>
        <h1 class="heading">Налоговый вычет</h1>
        <p class="summary">Используйте налоговый вычет чтобы погасить ипотеку досрочно. Размер налогового вычета составляет не более 13% от своего официального годового дохода.</p>
        <label class="salary-text" for="salary-input">Ваша зарплата в месяц</label>
        <input v-model="salary" type="text" class="salary-input" required placeholder="Введите данные" id="salary-input">
        <p v-if="isFilledErr" class="filled-err">Поле обязательно для заполнения</p>
        <button class="calc-btn" v-on:click="countTaxDeduction(260000)" :disabled="arrayOfDeductions.length ? true : false">Рассчитать</button>
        <div class="paymants-list">
            <p v-if="arrayOfDeductions.length" class="paymants-list-text">Итого можете внести в качестве досрочных:</p>
            <div class="paymants-list-item" v-for="(el, i) of arrayOfDeductions" :key="i">
                <input type="checkbox" class="custom-checkbox" :id="'checkbox' + i">
                <label :for="'checkbox' + i"></label>
                <p class="result">{{el}} рублей <span class="result-gray">за {{ i + 1 + "-" + declOfNum(i + 1)}} год</span> </p>
            </div>
        </div>
        <div class="what-decrease">
            <p class="what-decrease-text">Что уменьшаем?</p>
            <button class="what-decrease-btn" v-on:click="activePayment = !activePayment, activeTime = false" :class="{'what-decrease-btn-active': activePayment}">Платёж</button>
            <button class="what-decrease-btn" v-on:click="activeTime = !activeTime, activePayment = false " :class="{'what-decrease-btn-active': activeTime}">Срок</button>
        </div>
        <button class="add-btn" v-on:click="cleanForm">Добавить</button>
    </div>
</template>

<script>
    export default {
        data(){
            return{
                isFilledErr: false,
                salary: null,
                activePayment: false,
                activeTime: false,
                arrayOfDeductions: [],
                textForms: ["ый", "ой", "ий"]
            }
        },

        methods:{
            countTaxDeduction(sum){
                if(!this.salary || !Number.isInteger(+this.salary)) 
                    return this.isFilledErr = true;
                else this.isFilledErr = false;

                let persentOfSalary = (+this.salary * 12) * 0.13;
                if(sum < persentOfSalary){
                    this.arrayOfDeductions.push(new Intl.NumberFormat().format(sum));
                    return
                }
                let rest = sum - persentOfSalary;
                this.arrayOfDeductions.push(new Intl.NumberFormat().format(persentOfSalary));
                if(rest > persentOfSalary){
                    return this.countTaxDeduction(rest);
                } else {
                    this.arrayOfDeductions.push(new Intl.NumberFormat().format(rest));
                    return;
                }
            },

            declOfNum(num) {  
                let numLength = num.toString()[num.toString().length - 1];

                if((numLength == 2 || numLength == 6 || numLength == 7 || numLength == 8) &&
                    (num !== 12 || num !== 16 || num !== 17 || num !== 18)) 
                        return this.textForms[1]; 

                if(numLength == 3 && num !== 13) return this.textForms[2];

                return this.textForms[0]
            },

            cleanForm(){
                this.salary = "";
                this.arrayOfDeductions = [];
            }
        },

    }
</script>

<style scope>

    .txd{
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        max-width: 552px;
        padding: 27px 27px 32px 32px;
        box-sizing: border-box;
        border-radius: 30px;
        background: #fff;
    }

    .close{
        cursor: pointer;
        align-self: flex-end;
    }

    .heading{
        font-weight: 500;
        font-size: 27px;
        line-height: 34px;
    }

    .summary{
        font-size: 14px;
        line-height: 24px;
        color: #808080;
        margin: 16px 0 24px 0;
    }

    .salary-text{
        cursor: pointer;
        font-weight: 500;
        font-size: 14px;
        line-height: 24px;
    }

    .salary-input{
        align-self: stretch;
        border: 1px solid #DFE3E6;
        box-sizing: border-box;
        border-radius: 3px;
        font-size: 14px;
        line-height: 24px;
        padding: 8px 10px;
        box-sizing: border-box;
        margin: 8px 0 4px 0;
        outline: none;
    }

    .salary-input:hover{
        border: 1px solid #000000;
    }

    .salary-input-disabled{
        border: 1px solid #808080;
    }

    .salary-input-error{
        border: 1px solid #EA0029;
    }

    .filled-err{
        margin: 0 0 4px 0;
        font-size: 10px;
        line-height: 12px;
        color: #EA0029;
    }

    .calc-btn{
        cursor: pointer;
        font-weight: 500;
        font-size: 14px;
        line-height: 24px;
        color: #EA0029;
        background: none;
        outline: none;
        border: none;
    }

    .calc-btn:hover{
        color: #F53A31;
    }

    .calc-btn:active{
        color: #EA0029;;
    }

    .calc-btn:disabled{
        color: #808080;
        cursor: auto;
    }

    .paymants-list{
        margin: 16px 0 0 0;
    }

    .paymants-list-item{
        display: flex;
        padding: 11px 0;
        box-sizing: border-box;
        border-bottom: 1px solid #DFE3E6;
    }

    .paymants-list-text{
        font-weight: 500;
        font-size: 14px;
        line-height: 24px;
    }

    .result-gray{
        color: #808080;
    }

    .custom-checkbox{
        display: none;
    }

    .custom-checkbox+label {
        cursor: pointer;
    }

    .custom-checkbox+label::before {
        content: '';
        display: inline-block;
        width: 20px;
        height: 20px;
        background: #FFFFFF;
        border: 1px solid #DFE3E6;
        box-sizing: border-box;
        border-radius: 6px;
        background-repeat: no-repeat;
        background-position: center center;
        background-size: 50% 50%;
    }

    .custom-checkbox+label:hover::before{
        border-color: #000;
    }

    .custom-checkbox:checked+label::before {
        border-color: #0b76ef;
        background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56;
        background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 8 8'%3e%3cpath fill='%23fff' d='M6.564.75l-3.59 3.612-1.538-1.55L0 4.26 2.974 7.25 8 2.193z'/%3e%3c/svg%3e");
    }

    .result{
        font-size: 14px;
        line-height: 21px;
        margin: 0 0 0 11px
    }

    .what-decrease{
        display: flex;
        align-items: center;
        margin: 24px 0 40px 0;
    }

    .what-decrease-text{
        font-weight: 500;
        font-size: 14px;
        line-height: 24px;
        margin-right: 32px;
    }

    .what-decrease-btn{
        cursor: pointer;
        font-size: 14px;
        line-height: 24px;
        padding: 6px 12px;
        background: #EEF0F2;
        border-radius: 50px;
        border: none;
        margin-right: 16px;
    }

    .what-decrease-btn-active{
        background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56;
        border-radius: 50px;
        color: #fff;
    }

    .what-decrease-btn:hover{
        background: #DFE3E6;
    }

    .what-decrease-btn:active{
        background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56;
        border-radius: 50px;
        color: #fff;
    }

    .add-btn{
        cursor: pointer;
        background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56;
        box-shadow: 0px 0px 24px rgba(234, 0, 41, 0.33);
        border-radius: 6px;
        font-weight: 500;
        font-size: 16px;
        line-height: 24px;
        color: #fff;
        outline: none;
        border: none;
        align-self:stretch;
        padding: 16px 0;
        box-sizing: border-box;
    }

    .add-btn:hover{
        background: #EA0029;
    }

    .add-btn-disabled{
        background: #BEC5CC;
        box-shadow: none;
    }

    @media screen and (max-width: 768px) {
        .txd{
            max-width: 453px;
            padding: 27px 23px 16px 16px;
        }

        .svg-close{
            height: 18px;
            width: 18px;
        }

        .heading{
            font-weight: 500;
            font-size: 28px;
            line-height: 40px;
        }

        .summary{
            font-size: 14px;
            line-height: 24px;
        }

        .what-decrease{
            display: flex;
            align-items: center;
            margin: 24px 0 40px 0;
        }

        .what-decrease-text{
            margin-right: 32px;
        }

        .add-btn{
            font-size: 16px;
            line-height: 24px;
            padding: 16px 0;
        }
    }

    @media screen and (max-width: 320px) {
        .txd{
            padding: 22px 22px 16px 16px;
            border-radius: 0;
        }

        .svg-close{
            height: 18px;
            width: 18px;
        }

        .heading{
            font-weight: 500;
            font-size: 18px;
            line-height: 24px;
        }

        .summary{
            font-size: 12px;
            line-height: 16px;
        }

        .what-decrease{
            display: flex;
            align-items: center;
            flex-wrap: wrap;
            margin: 24px 0 25px 0;
        }

        .what-decrease-text{
            margin: 0 80px 24px 0;
        }

        .add-btn{
            font-size: 12px;
            line-height: 16px;
            padding: 12px 0;
        }

        
    }

</style>