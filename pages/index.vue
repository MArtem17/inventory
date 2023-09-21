<template>
    <div>
        <div class="left sceleton">
            <img class="left__img" src="Img.png" alt="image">
            <div class="left__data">
                <div style="position: absolute; left: 0px; top:0px; height: 26px; width: 190px; border-radius: 8px;"
                    class="data__item"></div>
                <div style="position: absolute; left: 17px; top:50px; height: 10px; width: 155px; border-radius: 4px;"
                    class="data__item"></div>
                <div style="position: absolute; left: 0px; top:76px; height: 10px; width: 190px; border-radius: 4px;"
                    class="data__item"></div>
                <div style="position: absolute; left: 10px; top:102px; height: 10px; width: 170px; border-radius: 4px;"
                    class="data__item"></div>
                <div style="position: absolute; left: 15px; top:128px; height: 10px; width: 160px; border-radius: 4px;"
                    class="data__item"></div>
                <div style="position: absolute; left: 25px; top:155px; height: 10px; width: 140px; border-radius: 4px;"
                    class="data__item"></div>
                <div style="position: absolute; left: 55px; top:188px; height: 10px; width: 80px; border-radius: 4px;"
                    class="data__item"></div>
            </div>
        </div>
        <div class="tasks__table">
            <div class="tasks__item" v-for="(item, i) in 25" :key="i" :id="i">
                <client-only>
                    <div v-if="inventory[i].img != null && !loaded">
                        <img :id="i" class="item__img" :src="inventory[i].img" alt="img-item" @click="openModalDelete(i)">
                        <div class="item__counter" @click="openModalCount(i)">
                            <span :id="i" class="counter__text">{{ inventory[i].count }}</span>
                        </div>
                    </div>
                </client-only>
            </div>
            <modal v-if="boolModal" class="modal" :inventory="element" :countBool="modalCount" @closeModal="closeModal()"
                @confirmCount="confirm($event)" @deleteInventory="deleteInventory()" />
        </div>
        <div class="bottom">
            <div class="bottom__data"></div>
            <svg class="bottom__close" width="12" height="12" viewBox="0 0 12 12" fill="none"
                xmlns="http://www.w3.org/2000/svg">
                <path
                    d="M12 1.05L10.95 0L6 4.95L1.05 0L0 1.05L4.95 6L0 10.95L1.05 12L6 7.05L10.95 12L12 10.95L7.05 6L12 1.05Z"
                    fill="white" />
            </svg>

        </div>
        <button class="button__reload" @click="reloadInventory()">Сбросить к исходным параметрам</button>
    </div>
</template>

<style>
body {
    font-family: "SF Pro Display", sans-serif;
    background-color: #1D1D1D;
}

.modal {
    position: absolute;
    top: 0px;
    right: 0px;
    animation: shineModal 1.5s 1;
}

@keyframes shineModal {
    0% {
        transform: translateX(+100%);
    }

    100% {
        transform: translateX(0%);
    }
}

.left {
    height: 500px;
    width: 236px;
    position: fixed;
    top: 32px;
    left: 32px;
    border-radius: 12px;
    border: 1px solid #4D4D4D;
    background-color: #262626;
}

.left__img {
    width: 208;
    height: 240;
    position: absolute;
    top: 18px;
    left: 14px;
    border-radius: 8px;
}

.left__data {
    height: 198px;
    width: 190px;
    position: absolute;
    top: 278px;
    left: 23px;
}

.data__item {
    overflow: hidden !important;
    background: #4D4D4D;
}

.data__item:after {
    content: "";
    position: absolute;
    height: 100%;
    width: 100%;
    z-index: 2;
    background: linear-gradient(90deg, #3C3C3C 0%, #444444 51.04%, #333333 100%);
    transform: translateX(-100%);
    animation: shine 1.5s infinite 0s;
}

.bottom {
    height: 72px;
    width: 785px;
    position: fixed;
    top: 556px;
    left: 32px;
    border-radius: 12px;
    border: 1px solid #4D4D4D;
    background-color: #262626;
}

.bottom__data {
    height: 36px;
    width: 699px;
    position: absolute;
    top: 18px;
    left: 18px;
    border-radius: 12px;
    overflow: hidden !important;
    background: #4D4D4D;
}

.bottom__data:after {
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

.bottom__close {
    position: absolute;
    top: 6px;
    right: 6px;
}

.tasks__table {
    width: 525px;
    height: 500px;
    position: fixed;
    top: 32px;
    left: 292px;
    border-radius: 12px;
    border: 1px solid #4D4D4D;
    background-color: #262626;
    display: flex;
    flex-wrap: wrap;
    overflow: hidden;
}

.tasks__item {
    transition: background-color 0.5s;
    text-align: center;
    border: 1px solid #4D4D4D;
    /* border-radius: 12px; */
    cursor: pointer;
    transition: background-color 0.5s;
    min-width: 103.6px;
    height: 98px;
    position: relative;
}

.item__counter {
    text-align: center;
    border: 1px solid #4D4D4D;
    border-radius: 6px 0px 0px 0px;
    width: 16px;
    height: 16px;
    position: absolute;
    bottom: 0px;
    right: 0px;
}

.counter__text {
    width: 8x;
    height: 12px;
    position: absolute;
    left: 4px;
    top: 2px;
    font-size: 10px;
    line-height: 12.1px;
    align-content: center;
    color: white;
    opacity: 0.4;

}

.item__img {
    width: 54px;
    height: 54px;
    position: absolute;
    left: 25px;
    top: 23px;
}

.selected {
    opacity: 0.6;
}

.button__reload {
    width: 500px;
    height: 40px;
    position: fixed;
    top: 650px;
    left: 160px;
    background: #FA7272;
    border-radius: 8px;
    border: 0px;
    color: white;
    font-size: 18px;
    line-height: 16.71px;
}

button:hover {
    cursor: pointer;
}
</style>

<script>
import modal from '~/components/modal.vue'
export default {
    components: { modal },
    data: () => ({
        inventory: [{ count: 4, img: "green.png" }, { count: 3, img: "yellow.png" }, { count: 2, img: "fiolet.png" }, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}],
        currentElement: null,
        nextElement: null,
        loaded: true,
        boolModal: false,
        modalCount: false,
        indexItemModal: null,
        element: { count: 4, img: "green.png" }
    }),
    beforeMount() {
        if (JSON.parse(localStorage.getItem("inventory")) != null) {
            this.inventory = []
            this.inventory = JSON.parse(localStorage.getItem("inventory"))
        }
        this.loaded = false
    },
    mounted() {
        if (!this.loaded) {
            const tasksListElement = document.querySelector(`.tasks__table`)
            const taskElements = tasksListElement.querySelectorAll(`.tasks__item`)

            for (const task of taskElements) {
                task.draggable = true
            }
            tasksListElement.addEventListener(`dragstart`, (evt) => {
                evt.target.classList.add(`selected`)
                this.currentElement = evt.target.id
            })

            tasksListElement.addEventListener(`dragend`, (evt) => {
                evt.target.classList.remove(`selected`)
                let el = this.inventory[this.currentElement]
                let i = 0
                if (Number(this.currentElement) < Number(this.nextElement)) {
                    console.log(this.inventory[1])
                    for (i = Number(this.currentElement); i < Number(this.nextElement) - 1; i++) {
                        this.inventory[i] = this.inventory[i + 1]
                    }
                    this.inventory[i] = el
                    localStorage.setItem("inventory", JSON.stringify(this.inventory))
                    location.reload()
                } else if (Number(this.currentElement) > Number(this.nextElement)) {
                    for (i = Number(this.currentElement); i > Number(this.nextElement); i--) {
                        this.inventory[i] = this.inventory[i - 1]
                    }
                    this.inventory[i] = el
                    localStorage.setItem("inventory", JSON.stringify(this.inventory))
                    location.reload()
                }
            });
            tasksListElement.addEventListener(`dragover`, (evt) => {
                evt.preventDefault();

                const activeElement = tasksListElement.querySelector(`.selected`)
                const currentElement = evt.target
                const isMoveable = activeElement !== currentElement &&
                    currentElement.classList.contains(`tasks__item`)

                if (!isMoveable) {
                    return;
                }

                const nextElement = (currentElement === activeElement.nextElementSibling) ?
                    currentElement.nextElementSibling :
                    currentElement
                this.nextElement = nextElement.id
                tasksListElement.insertBefore(activeElement, nextElement)
            })
        }
    },
    methods: {
        closeModal() {
            this.boolModal = false
        },
        openModalCount(i) {
            this.modalCount = true
            this.element = this.inventory[i]
            this.indexItemModal = i
            this.boolModal = true
        },
        openModalDelete(i) {
            this.modalCount = false
            this.element = this.inventory[i]
            this.indexItemModal = i
            this.boolModal = true
        },
        confirm(data) {
            this.inventory[this.indexItemModal].count = Number(data.count)
            localStorage.setItem('inventory', JSON.stringify(this.inventory))
        },
        deleteInventory() {
            this.inventory[this.indexItemModal] = {}
            localStorage.setItem('inventory', JSON.stringify(this.inventory))
            this.boolModal = false
        },
        reloadInventory() {
            const inventory = [{ count: 4, img: "green.png" }, { count: 3, img: "yellow.png" }, { count: 2, img: "fiolet.png" }, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}]
            localStorage.setItem('inventory', JSON.stringify(inventory))
            this.inventory = inventory
        }
    }
}
</script>