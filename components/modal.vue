<template>
    <div class="modal">
        <div class="modal__close" @click="$emit('closeModal')">
            <svg width="12" height="12" viewBox="0 0 12 12" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path
                    d="M12 1.05L10.95 0L6 4.95L1.05 0L0 1.05L4.95 6L0 10.95L1.05 12L6 7.05L10.95 12L12 10.95L7.05 6L12 1.05Z"
                    fill="white" />
            </svg>
        </div>
        <img class="modal__img" :src="inventory.img" alt="img-item">
        <hr class="modal__divider" style="top:215px">
        <div style="position: absolute; left: 19px; top:237px; height: 30px; width: 211px; border-radius: 8px;"
            class="modal__data"></div>
        <div style="position: absolute; left: 19px; top:291px; height: 10px; width: 211px; border-radius: 4px;"
            class="modal__data"></div>
        <div style="position: absolute; left: 19px; top:316px; height: 10px; width: 211px; border-radius: 4px;"
            class="modal__data"></div>
        <div style="position: absolute; left: 19px; top:341px; height: 10px; width: 211px; border-radius: 4px;"
            class="modal__data"></div>
        <div style="position: absolute; left: 35px; top:366px; height: 10px; width: 180px; border-radius: 4px;"
            class="modal__data"></div>
        <div style="position: absolute; left: 85px; top:391px; height: 10px; width: 80px; border-radius: 4px;"
            class="modal__data"></div>
        <hr class="modal__divider" style="top:424px">
        <button class="modal__delete" @click="$emit('deleteInventory')">Удалить предмет</button>
        <div v-if="countBool" class="modal__count">
            <input class="count__input" v-model="count" placeholder="Введите количество" type="text">
            <button class="count__clear" @click="clear()">Отмена</button>
            <button class="count__confirm" @click="confirm()">Подтвердить</button>
        </div>
    </div>
</template>

<style>
.modal {
    width: 250px;
    height: 500px;
    border: 1px solid #4D4D4D;
    border-radius: 0px 12px 12px 0px;
    background-color: #262626;
    opacity: 0.8;
    position: relative;
}

.modal__close {
    position: absolute;
    width: 12;
    height: 12;
    top: 8px;
    right: 8px;
}

.modal__close:hover {
    cursor: pointer;
}

.modal__img {
    width: 130px;
    height: 120px;
    position: absolute;
    top: 55px;
    left: 60px;
}

.modal__divider {
    width: 220px;
    position: absolute;
    left: 15px;
    color: #4D4D4D;
}

.modal__data {
    overflow: hidden !important;
    background: #4D4D4D;
}

.modal__data:after {
    content: "";
    position: absolute;
    height: 100%;
    width: 100%;
    z-index: 2;
    background: linear-gradient(90deg, #3C3C3C 0%, #444444 51.04%, #333333 100%);
    transform: translateX(-100%);
    animation: shine 1.5s infinite 0s;
}

@keyframes shine {
    0% {
        transform: translateX(-100%);
    }

    100% {
        transform: translateX(100%);
    }
}

.modal__delete {
    width: 220px;
    height: 39px;
    position: absolute;
    top: 443px;
    left: 15px;
    background: #FA7272;
    border-radius: 8px;
    border: 0px;
    color: white;
    font-size: 14px;
    line-height: 16.71px;
}

.modal__count {
    width: 250px;
    height: 133px;
    position: absolute;
    z-index: 3;
    left: 0px;
    bottom: 0px;
    border: 1px solid #4D4D4D;
    border-radius: 0px 0px 12px 0px;
    background-color: #262626;
}

.count__input {
    width: 210px;
    height: 40px;
    position: absolute;
    top: 20px;
    left: 20px;
    border: 1px solid #4D4D4D;
    border-radius: 4px;
    background-color: #262626;
    color: white;
}

.count__clear {
    background: white;
    width: 88px;
    height: 33px;
    position: absolute;
    top: 80px;
    left: 20px;
    border-radius: 8px;
    border: 0px;
    color: #2D2D2D;
    font-size: 14px;
    line-height: 16.71px;
}

.count__clear:before {
    width: 92px;
    height: 37px;
    position: absolute;
    content: '';
    top: -2px;
    left: -2px;
    z-index: -1;
    background: #ff9999;
    border-radius: 8px;
    border: 0px;
    filter: blur(5px);
}

.count__confirm {
    width: 115px;
    height: 33px;
    position: absolute;
    top: 80px;
    left: 120px;
    background: #FA7272;
    border-radius: 8px;
    border: 0px;
    color: white;
    font-size: 14px;
    line-height: 16.71px;
}

.count__confirm:before {
    width: 119px;
    height: 37px;
    position: absolute;
    content: '';
    top: -2px;
    left: -2px;
    z-index: -1;
    background: #ff9999;
    border-radius: 8px;
    border: 0px;
    filter: blur(5px);
}

button:hover {
    cursor: pointer;
    background: #ff8b8b;
}

.count__clear:hover {
    background: #bababa;
}
</style>

<script>
export default {
    props: {
        inventory: Object,
        countBool: Boolean
    },
    data: () => ({
        count: null
    }),
    watch: {
        count(newVal, oldVal) {
            if (!/[0-9]/.test(newVal)) {
                this.count = null
            }
            if (/[a-z]|[A-Z]/.test(newVal)) {
                this.count = oldVal
            }
        }
    },
    methods: {
        clear() {
            this.count = null
        },
        confirm() {
            if (this.count != null && !/[a-z]|[A-Z]/.test(this.count)) {
                this.$emit('confirmCount', { count: this.count })
            }
        },
    }
}
</script>