<template>
    <div class="inventory__info-panel">
        <button @click="closeInfoPanel" class="inventory__close-btn">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 384 512">
                <path d="M376.6 84.5c11.3-13.6 9.5-33.8-4.1-45.1s-33.8-9.5-45.1 4.1L192 206 56.6 43.5C45.3 29.9 25.1 28.1 11.5 39.4S-3.9 70.9 7.4 84.5L150.3 256 7.4 427.5c-11.3 13.6-9.5 33.8 4.1 45.1s33.8 9.5 45.1-4.1L192 306 327.4 468.5c11.3 13.6 31.5 15.4 45.1 4.1s15.4-31.5 4.1-45.1L233.7 256 376.6 84.5z"/>
            </svg>
        </button>
        <div class="info-panel__img" :style="`height: ${isFirstType ? '40%' : '35%'}`">
            <div class="item__draggable" :style="`background: ${item.color}`">
                <p class="draggable__count">{{ item.count}}</p>
            </div>
        </div>
        <hr>
        <div class="info-panel__description">
            <h2>Объект</h2>
            <p>Lorem ipsum, dolor sit amet consectetur adipisicing elit. Expedita error necessitatibus natus. Ullam perspiciatis repudiandae distinctio autem, in laudantium eius. Lorem ipsum dolor sit amet consectetur, adipisicing elit. Harum architecto est cupiditate consequatur? Sapiente voluptate quibusdam atque id praesentium quod, itaque dolore soluta odit eum doloremque nemo quam suscipit modi?</p>
        </div>
        <hr>
        <div class="info-panel__options">
            <div v-if="isFirstType">
                <button @click="firstDelStage">Удалить предмет</button>
            </div>
            <div v-else>
                <input type="text" placeholder="Введите количество" @input="checkCorrectInput" v-model="inputItemCount">
                <div class="optios__btn-block">
                    <button @click="backDelStage">Отменить</button>
                    <button @click="delDragItem">Подтвердить</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'InventoryInfo',
    props: {
        item: Object
    },
    data() {
        return {
            isFirstType: true,
            inputItemCount: ''
        }
    },
    methods: {
        closeInfoPanel() {
            this.$emit('update-info-panel-state', false);
            this.isFirstType = true;
        },
        firstDelStage() {
            this.isFirstType = false;
        },
        backDelStage() {
            this.isFirstType = true;
        },
        delDragItem() {
            this.$store.dispatch( 'deleteDragItem', {
                id: this.item.id,
                count: this.inputItemCount
            });
            this.isFirstType = true;
            this.$emit('update-info-panel-state', false);
            this.inputItemCount = '';
        },
        checkCorrectInput( event ) {
            if ( !(/^\d+$/.test( this.inputItemCount ) ) ) {
                this.inputItemCount = this.inputItemCount.slice(0, -1);
            }
            
            if ( Number(this.inputItemCount) > this.item.count ) {
                this.inputItemCount = this.inputItemCount.slice(0, -1);
            }
        }
    }
}
</script>

<style lang="scss">
.inventory__info-panel {
    position: absolute;
    width: 50%;
    height: 100%;
    min-height: 600px;
    backdrop-filter: blur(10px);
    background: transparent;
    border: 2px solid #4D4D4D;
    top: 0;
    transition: all 0.5s ease;
    z-index: 100000;

    @media ( max-width: 425px ) {
        width: 60%;
    }

    .inventory__close-btn {
        position: absolute;
        width: 25px;
        height: 25px;
        background: transparent;
        overflow: hidden;
        top: 10px;
        right: 10px;
        border: 0;

        svg {
            width: 15px;

            path {
                fill: white
            }
        }
    }

    .info-panel__img {
        display: flex;
        justify-content: center;
        align-items: center;

        .item__draggable {
            position: relative;
            width: 120px;
            height: 120px;

            &::after {
                content: '';
                position: absolute;
                left: -10px;
                top: 10px;
                width: 100%;
                height: 100%;
                box-shadow: 2px -2px 5px white;
            }

            .draggable__count {
                position: absolute;
                text-align: center;
                bottom: -10px;
                right: 0;
                width: 20px;
                height: 20px;
                background: rgb(255, 255, 255);
                box-shadow: 0 0 3px rgb(80, 80, 80) inset;
                border-radius: 12px 0 0 0;
                color: rgb(0, 0, 0);
                z-index: 10000;
            }
        }
    }

    .info-panel__description {
        padding: 20px;
        max-height: 45%;
        display: flex;
        flex-direction: column;
        gap: 25px;
        overflow-y: auto;

        h2 {
            font-size: 30px;
            text-align: center;
            color: #a0a0a0;
        }

        p {
            font-size: 16px;
            color: #a0a0a0;
        }
    }

    .info-panel__options {
        width: 100%;
        min-height: 10%;
        padding: 10px 20px;

        div:first-of-type {
            width: 100%;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;

            button {
                width: 80%;
                height: 39px;
                border-radius: 10px;
                background: #FF8888;
                color: white;
                transition: all 0.2s linear;
    
                &:hover {
                    scale: 1.05;
                }
            }
        }

        div:last-of-type {
            width: 100%;
            height: 100%;
            display: flex;
            gap: 10px;
            flex-direction: column;
            justify-content: center;
            align-items: center;

            input {
                width: 100%;
                height: 40px;
                border-radius: 5px;
                background: transparent;
                padding: 3px 10px;
            }

            .optios__btn-block {
                display: flex;
                flex-direction: row;

                button {
                    height: 39px;
                    border-radius: 10px;
                    transition: all 0.2s linear;
        
                    &:hover {
                        scale: 1.05;
                    }
    
                    &:first-of-type {
                        background: white;
                        color: black;
                    }
    
                    &:last-of-type {
                        background: #FF8888;
                        color: white;
                    }
                }
            }
        }
    }
}
</style>