<template>
    <div class="unloadcard block" :class="status" @click="$emit('id', props.item.id)">
        <div>Задача выполнена: <span class="text-bold">{{ props.item.date }}</span> </div>
        <div>Статус задачи: <span class="text-bold">{{ props.item.status_text }}</span> </div>
        <div>ID выгрузки: <span class="text-bold">{{ props.item.id }}</span></div>
        <div>Выгрузка заказа из фотосессии: <span class="text-bold">{{ event }}</span></div>
        <div>Размер выгрузки: <span class="text-bold">>{{ props.item.size }}</span></div>

    </div>
        
</template>

<script setup>
    const props = defineProps({
        item: {
        type: Object,
        required: true
        },
    });
    const event = computed(() => {
        const text = props.item.event.replace(/(<([^>]+)>)/gi, '');
        const index = text.lastIndexOf('№');
        return text.slice(index);
    })
    const status = computed(() => {
        if (props.item.status === 'green') {
            return 'unloadcard_success'
        } else if (props.item.status === 'red') {
            return 'unloadcard_danger'
        }
    });
</script>

<style lang="scss">
.unloadcard {
    padding: 15px;
    font-size: 14px;
    border-left: 10px solid;
    border-radius: 3px;
    margin-bottom: 15px;
    cursor: pointer;
    &_success {
        border-left: 10px solid var(--color_success);
    }
    &_danger {
        border-left: 10px solid var(--color_danger);
    }
}
</style>