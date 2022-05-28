<template>
    <div class="card" @mouseover="showCart(cart_item)" @mouseleave="hideCart(cart_item)">
        <div class="card__images">
            <img :src="cart_item.images" :alt="cart_item.images">
        </div>
        <div class="card__content">
            <div class="card__name">
                <h3 class="card__name-item">{{cart_item.name}}</h3>
            </div>
            <div class="card__description">
                <p class="card__description-item">{{cart_item.content}}</p>
            </div>
            <div class="card__price">
                <p class="card__price-item">{{cart_item.price.toString().replace(/(\d)(?=(\d\d\d)+(?!\d))/g, '$1 ')}} руб.</p>
            </div>
        </div>
        <img v-show="showCartIcon" @click="removeItem(cart_item)" class="card__delete-basket" src="../../assets/images/basket-delete.svg" alt="../../assets/images/basket-delete.svg">
    </div>
</template>

<script>
export default {
    name : 'card',
    props : {
        cart_item : {
            type : Object,
            default () {
                return {}
            }
        }
    },

    data() {
        return {
            showCartIcon : false,
        }
    },

    methods : {
        showCart : function() {
            this.showCartIcon = true;
        },
        hideCart : function() {
            this.showCartIcon = false;
        },
        removeItem : function(item) {
            this.$emit('removeCartItem', item);
        }
    }
}
</script>

<style lang="scss" scoped>

$col-text-1: #3F3F3F;
$col-text-2: #B4B4B4;
$col-text-3: #49485E;

p, h3 {
    margin : 0px;
}

.card {
    max-width: 332px;
    min-height : 423px;
    display : flex;
    flex-direction: column;
    border-radius: 4px;
    position: relative;
    background: #FFFEFB;
    margin-bottom: 16px;
    box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
    cursor: pointer;
    &__images {
        display : flex;
        max-width: 332px;
        height: 200px;
        border-top-right-radius: 4px;
        border-top-left-radius: 4px;
        img {
            width: 100%;
        }
    }
    &__content {
        padding: 16px;
        max-width: 300px;
        background: #FFFEFB;
        display : flex;
        flex-direction: column;
    }
    &__name {
        -webkit-line-clamp: 1;
        -webkit-box-orient: vertical;
        overflow: hidden;
        text-overflow: ellipsis;
        max-height: 26px;
    }
    &__name-item {
        margin-top: 1px;
        color: $col-text-1;
        letter-spacing: 0.75px;
        display : flex;
        flex-wrap: nowrap;
        font-family: 'Source Sans Pro';
    }
    &__description {
        display : flex;
        color: $col-text-1;
        font-weight: 400;
        font-size: 16px;
        line-height: 20px;
        margin-top: 16px;
        height: 79px;
        overflow: hidden;
    }
    &__description-item {
        font-family: 'Source Sans Pro';
    }
    &__price {
        margin-top: 31px;
    }
    &__price-item {
        font-weight: 600;
        font-size: 24px;
        line-height: 30px;
        color: #3F3F3F;
        font-family: 'Source Sans Pro';
        margin-top: 4px;
    }
    &__delete-basket {
        position : absolute;
        right: 0;
        right: -13px;
        top: -10px;
    }
}

@media screen and (max-width : 1130px) {
    .card {
        width: 48%;
    }
}

@media screen and (max-width : 950px) {
    .card {
        width: 100%;
        margin : 0 auto;
        margin-bottom: 20px;
    }
}

@media screen and (max-width : 570px) {
    .card {
        width: 49%;
    }
}

@media screen and (max-width : 480px) {
    .card {
        width: 100%;
    }
}
</style>