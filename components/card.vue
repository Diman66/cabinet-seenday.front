<template>
    <div class="card block"> 
        <div class="card__title text-bold">
            <span><slot name="header"></slot></span>
        </div>
        <slot></slot>
    </div>
</template>

<style lang="scss">
    .card {
        padding: 15px;
        &__title {
            font-size: 18px;
            @include start-at("xslg") {
                font-size: 20px;
            }
        }
    }
</style>
