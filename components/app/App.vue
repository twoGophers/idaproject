<template>
    <div class="basket">
        <header class="basket__header">
            <h2 class="basket__title">Добавдение товара</h2>
            <div class="basket__sort">
                <div class="basket__sort-text" @click="clickFilter = !clickFilter">
                    <p class="basket__button">По умолчанию</p>
                    <img class="basket__arrow" src="../../assets/images/Rectangle33.svg" alt="../../assets/images/Rectangle33.svg">
                </div>
                <div v-if="clickFilter" class="basket__man">
                    <p @click="filterName">Имя</p>
                    <p @click="filterMin">Min</p>
                    <p @click="filterMax">Max</p>
                </div>
            </div>
        </header>
        <main class="basket__main">
            <form @submit="onCheckForm" v-if="showFormItem" class="basket__form form">
                <div class="form__name">
                    <label class="form__label" for="name">Наименование товара</label>
                    <input class="form__form-input" v-model="modalName" type="text" placeholder="Введите наименование товара" name="name" id="">
                    <label class="form__label-valid" :style="[modalName ? {opacity : 0} : {opacity : 1}]" for="name">{{errorName}}</label>
                </div>
                <div class="form__description">
                    <label class="form__label" for="description">Описание товара</label>
                    <textarea class="form__form-textarea" v-model="modalContent" type="text" placeholder="Введите описание товара" name="description" id=""></textarea>
                    <label class="form__label-valid" for="description">{{errorContent}}</label>
                </div>
                <div class="form__link">
                    <label class="form__label" for="link">Ссылка на изображение товара</label>
                    <input class="form__form-input" v-model="modalImages" type="text" placeholder="Введите ссылку" name="link" id="">
                    <label class="form__label-valid" :style="[modalImages ? {opacity : 0} : {opacity : 1}]" for="link">{{errorImages}}</label>
                </div>
                <div class="form__price">
                    <label class="form__label" for="price">Цена товара</label>
                    <input class="form__form-input" v-model="modalPrice" type="text" placeholder="Введите цену" name="price" id="">
                    <label class="form__label-valid" :style="[modalPrice ? {opacity : 0} : {opacity : 1}]" for="price">{{errorPrice}}</label>
                </div>
                <button class="form__button" :style="[isButtonDisabled ? {background : '#EEEEEE', color : '#B4B4B4'} : {background : '#7BAE73', color : '#FFFFFF'}]" type="submit" :disabled="isButtonDisabled">Добавить товар</button>
            </form>
            <div class="basket__card list-card">
                <Card
                    v-for="item in this.activeCart"
                    :key="item.id"
                    :cart_item="item"
                    @removeCartItem="removeCartItem"
                    >
                </Card>
            </div>
        </main>
    </div>
</template>

<script>
import Card from '../card/Card.vue';
export default {
    name : 'basket',
    components : {
        Card
    },
    data() {
        return {
            List : [
                { 
                    id : 1,
                    status : null,
                    name : 'Товар',
                    content : 'Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строк',
                    images : 'https://natworld.info/wp-content/uploads/2018/01/%D0%A1%D0%BE%D1%87%D0%B8%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5-%D0%BD%D0%B0-%D1%82%D0%B5%D0%BC%D1%83-%D0%9F%D1%80%D0%B8%D1%80%D0%BE%D0%B4%D0%B0.jpeg',
                    price : 10000
                },
                { 
                    id : 2,
                    status : null,
                    name : 'Наименование товара',
                    content : 'Довольно-таки ',
                    images : 'https://icocnews.ru/wp-content/uploads/2015/09/priroda.jpg',
                    price : 10000
                },
                { 
                    id : 3,
                    status : null,
                    name : 'Наименование товара',
                    content : 'Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строк',
                    images : 'https://natworld.info/wp-content/uploads/2018/01/%D0%A1%D0%BE%D1%87%D0%B8%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5-%D0%BD%D0%B0-%D1%82%D0%B5%D0%BC%D1%83-%D0%9F%D1%80%D0%B8%D1%80%D0%BE%D0%B4%D0%B0.jpeg',
                    price : 10000
                },
                { 
                    id : 4,
                    status : null,
                    name : 'Наименование товара',
                    content : 'Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строк',
                    images : 'https://natworld.info/wp-content/uploads/2018/01/%D0%A1%D0%BE%D1%87%D0%B8%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5-%D0%BD%D0%B0-%D1%82%D0%B5%D0%BC%D1%83-%D0%9F%D1%80%D0%B8%D1%80%D0%BE%D0%B4%D0%B0.jpeg',
                    price : 1000
                },
            ],
            modalName : '',
            modalContent : '',
            modalImages : '',
            modalPrice : '',
            //error
            errorName : '',
            errorContent : '',
            errorImages : '',
            errorPrice : '',

            //Button
            isButtonDisabled : true,

            activeCart : [],

            //Filter
            clickFilter : false,

            //Form widht < 550px
            showFormItem : true,
            width : window.innerWidth,
            height : window.innerHeight
        }
    },
        methods : {
        onCheckForm: function(e) {
            e.preventDefault();
            this.activeCart.push({
            id : this.activeCart.index,
            name : this.modalName,
            content : this.modalContent,
            images : this.modalImages,
            price : this.modalPrice
            })
            this.modalName = '';
            this.modalContent = '';
            this.modalImages = '';
            this.modalPrice = '';

            console.log(this.activeCart);

            this.saveList();  
        },

        validateModalName(value) {
            if(value == '') {
                this.errorName = 'Поле является обязательным';
            } else {
                this.errorName= '';
                this.modalName = value;
            }
        },
        validateModalContent(value) {
            if(value == '') {
                this.errorContent = 'Поле является обязательным';
            } else {
                this.errorContent = '';
            }
        },
        validateModalImages(value) {
            if(value == '') {
                this.errorImages = 'Поле является обязательным';
            } else {
                this.errorImages = '';
            }
        },
        validateModalPrice(value) {
            if(value == '') {
                this.errorPrice = 'Поле является обязательным';
            } else {
                this.errorPrice = '';
            }
        },
//Button fotm
        showBtn: function() {
            if(this.modalName && this.modalContent && this.modalImages && this.modalPrice) {
                this.isButtonDisabled = false;
            } else {
                this.isButtonDisabled = true;
            }
        },
//Remove cart item from catalog
        removeCartItem(itemCart) {
            this.activeCart.map(item => {
                let index = this.activeCart.indexOf(item);
                if(item == itemCart) {
                    this.activeCart.splice(index, 1);
                    this.saveList();
                } else {
                    return this.activeCart;
                }
            })
        },

//Save List
        saveList : function() {
            const parsed = JSON.stringify(this.activeCart);
            localStorage.setItem('List', parsed);
        },

//Show filter
        showFilter : function() {
            this.clickFilter = true;
        },
//Filter Name
        filterName : function() {
            this.activeCart = this.activeCart.sort((a, b) => a.name.localeCompare(b.name));
        },
//Filter Min
        filterMin : function() {
            this.activeCart.sort((prev, curr) => prev.price - curr.price);
        },
//Filter Max
        filterMax : function() {
            this.activeCart.sort((prev, curr) => curr.price - prev.price);
        },
// width and height
        // onResize() {
        //     this.width = window.innerWidth;
        //     this.height = window.innerHeight;
        //     if(this.width < 550) {
        //         this.showFormItem = false;
        //     } else {
        //         this.showFormItem = true;
        //     }
        // },
    },
    watch : {
        modalName(value) {
            this.modalName = value;
            this.showBtn();
            this.validateModalName(value);
        },
        modalContent(value) {
            this.modalContent = value;
            this.showBtn();
            this.validateModalContent(value);
        },
        modalImages(value) {
            this.modalImages = value;
            this.showBtn();
            this.validateModalImages(value);
        },
        //??Number
        modalPrice(value) {
            let test = this.modalPrice;
            value = test.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ");
            this.showBtn();
            this.validateModalPrice(value);
        },
    },

    mounted() {
        this.activeCart = this.List;
        if(localStorage.getItem('List')) {
            try {
                this.activeCart = JSON.parse(localStorage.getItem('List'));
            } catch(e) {
                localStorage.removeItem('List');
            }
        }
    },
//winth and heigth
    created() {
        window.addEventListener("resize", this.onResize);
    },

    destroyed() {
        window.removeEventListener("resize", this.onResize);
    },
}
</script>

<style lang="scss" scoped>
$box-shadow-button : 0px 2px 5px rgba(0, 0, 0, 0.1);
$box-shadow-block: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
$box-shadow-input: 0px 2px 5px rgba(0, 0, 0, 0.1);
$border-radius : 4px;

//Style
$font-family :'Source Sans Pro', sans-serif;

$fsn : normal;

$fw-600 : 600;
$fw-400 : 400;

$lh-35px : 35px;
$lh-15px : 15px;
$lh-13px : 13px;
$lh-10px : 10px;

$fz-28: 28px;
$fz-12: 12px;
$fz-11: 11px;
$fz-10: 10px;
$fz-9: 9px;
$fz-8: 8px;

$col-text-1: #3F3F3F;
$col-text-2: #B4B4B4;
$col-text-3: #49485E;
$col-text-valid:#FF8484;

@mixin font ($fz, $fw, $lh, $fsn, $col-text) {
    font-size: $fz;
    font-weight: $fw;
    line-height: $lh;
    font-style: $fsn;
    color : $col-text;
}

//Display
$dsf : flex;
$dfr : row;
$djc : column;
$djc-s : flex-start;
$djc-c : center;
$djc-e : flex-end;
$djc-sb : space-between;
$dai-s : flex-start;
$dai-c : center;
$dai-e : flex-end;

@mixin display ($ds, $fd, $jc, $ai) {
    display : $ds;
    flex-direction: $fd;
    justify-content: $jc;
    align-items: $ai;
}

textarea {
    resize:none
}

button:focus {
    border: 0px !important;
    outline: 0!important;
}

input:focus,
textarea:focus {
    outline:none;
}

.basket {
    margin : 0px 32px;
    &__header {
        @include display ($dsf, $dfr, $djc-sb, $dai-s);
        height: 75px;
    }
    &__main {
        display : flex;
        flex-direction: row;
    }
    &__title {
        @include font($fz-28, $fw-600, $lh-35px, $fsn, $col-text-1);
        letter-spacing: -0.9px;
    }
    &__sort {
        width: 121.49px;
        height: 36px;
        margin-top: 23px;
        box-shadow: $box-shadow-button;
        border-radius: $border-radius;
        background: #FFFEFB;
        position: relative;
    }
    &__sort-text {
        @include display ($dsf, $dfr, $djc-c, $dai-c);
        margin-top: -2px;
        cursor: pointer;
    }
    &__button {
        @include font($fz-12, $fw-400, $lh-15px, $fsn, $col-text-2);
        margin-left: 1px;
    }

    &__man{
        position: absolute;
        width: 100%;
        left: 0px;
        top: 40px;
        background: #FFFEFB;
        box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
        border-radius: 10px;
        z-index: 1000;
        // display: none;
        display : flex;
        flex-direction: column;
        transition: 0.5s;
        overflow: hidden;
        p {
            height: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.185);
            font-family: $font-family ;
            color: #B4B4B4;
            font-size: 12px;
            font-weight: 400;
            line-height:15px;
            margin: 1%;
            transition: 0.5s;
            cursor: pointer;
            &:hover {
                background: #e2e2e2;
                color: white;
            }
        }
    }

    .form {
        width: 284px;
        height : 386px;
        box-shadow : $box-shadow-block;
        background: #FFFEFB;
        border-radius: $border-radius;
        padding: 30px 24px;
        display : flex;
        flex-direction: column;
        &__name,
        &__description,
        &__link, 
        &__price {
            display : flex;
            flex-direction: column;
        }
        &__description{
            margin-top: 12px;
        }
        &__link{
            margin-top:2px
        }
        &__price {
            margin-top: 12px
        }
        &__label {
            @include font($fz-9, $fw-400, $lh-13px, $fsn, $col-text-3);
            letter-spacing: 0.012em;
            margin-top: -6px;
        }
        &__form-input::placeholder {
            @include font($fz-10, $fw-400, $lh-15px, $fsn, $col-text-2);
        }
        &__form-input {
            height : 16px;
            box-shadow : $box-shadow-input;
            border-radius: $border-radius;
            border: none;
            padding : 13px 16px 11px 16px;
            @include font($fz-10, $fw-400, $lh-15px, $fsn, $col-text-1);
            letter-spacing: 0.75px;
        }
        &__form-textarea::placeholder {
            @include font($fz-11, $fw-400, $lh-15px, $fsn, $col-text-2);
        }
        &__form-textarea {
            height: 82px;
            box-shadow: $box-shadow-input;
            border-radius: $border-radius;
            padding: 15px 16px;
            @include font($fz-11, $fw-400, $lh-15px, $fsn, $col-text-1);
            letter-spacing: -0.2px;
            border: none;
            margin-bottom: 10px
        }
        &__label-valid {
            @include font($fz-8, $fw-400, $lh-10px, $fsn, $col-text-valid);
            letter-spacing: -0.02em;
            height: 10px;
        }
        &__button {
            width: 284px;
            height: 36px;
            background : #EEEEEE;
            border-radius: 10px;
            border : none;
            margin-top: 15px;
            letter-spacing: -0.5px;
            cursor: pointer;
        }
    }
    .list-card {
        width: 1028px;
        margin-left: 16px;
        display : flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: space-between;
    }
}

@media screen and (max-width : 650px) {
    .basket {
        margin : 0px 10px;
        .form {
            width: 49%;
        }
        .list-card {
            width: 49%;
        }
    }
}

@media screen and (max-width : 570px) {
    .basket {
        margin : 0px 10px;
        &__main {
            flex-direction: column;
        }
        .form {
            width: 94%;
            padding : 3%;
        }
        .form__button {
            margin: 0 auto;
            width: 70%;
        }
        .list-card {
            margin-top: 20px;
            width: 100%;
            margin-left: 0px;
        }
    }
}

@media screen and (max-width : 430px) {
 .basket {
     &__header {
         flex-direction: column;
         height: fit-content;
     }
     &__sort {
        margin-top: 0px;
        margin-bottom: 20px;
     }
 }
}
</style>