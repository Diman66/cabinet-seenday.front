<template>
    <main>
        <div class="unload">
            <card class="unload__card">
                <template v-slot:header>Выгрузка</template>
                <!-- Выполняет работу:
                - Собирает фотографии из заказов пользователей.
                - Выгружает по папкам -->
                <template v-slot:default>
                    <div class="body">
                        <div class= "body__head text-bold">Выполняет работу:</div>
                        <ul class = "body__list">
                            <li> Собирает фотографии из заказов пользователей.</li>
                            <li> Выгружает по папкам.</li>
                        </ul>
                    </div>
                    
                </template>
            </card>
            <div class="unload__list">
                <p v-if="pending">Загрузка данных...</p>
                <pre v-else-if="error">Данные не загрузились: {{ error.data }}</pre>
                <template v-else>
                    <unloadcard v-for="item of unloadData.data" :key="item.id" :item="item" @id="(id)=>itemCheckedId=id" />
                </template>
            </div>
            <div class="unload__info" >
                <div v-if="itemChecked" class="info__linkBlock block">
                    <h3 class="linkBlock__head "><span class="text-bold">Ссылка для скачивания архива Выгрузки (.zip):</span></h3>
                    <a class="link" href="">{{ itemChecked.download_link }}</a>
                    <span class="linkBlock__button span-link" @click="copyLink(itemChecked.download_link)">скопировать ссылку</span>
                </div>
                <div v-else class="notice info__block" data-color="light-purple">
                    <p>Для того чтобы просмотреть информацию о <span class="text-bold">выгрузке</span>, а также ее скачать, нажмите на требуюмую выгрузку в столбце слева.</p>
                </div>
            </div>
        </div>
    </main>
</template>

<script setup>
    // API для получения списка выгрузки -
    // https://dev-cabinet.seenday.com/e.scripts?page=pages:unload&event=get
    // API для получения информации по 1 выгрузке -
    // https://dev-cabinet.seenday.com/e.scripts?page=pages:unload&event=get&unload_i
    // d=2175
    const itemCheckedId = ref('');
    const itemChecked = ref()
    const API = 'https://dev-cabinet.seenday.com/e.scripts?page=pages:unload&event=get'
    
    const unloadData = computed(() => {
        const response = JSON.parse(res.value)
        return response.response
    })

    watch(itemCheckedId, async (newItem) => {
        await fetchItem(newItem) 
    })
    
    const {data: res, pending} =  await useAPIFetch(API);

    async function fetchItem (id = '') {
        try {
            const options = {
            params: {
                unload_id: id
            }
            }
            let {data: response } =  await useAPIFetch(API, options);
            response = JSON.parse(response.value);
            itemChecked.value = response.response.data[0];
        }
        catch (e) {

        }
    }
    function copyLink (link) {
        try {
            navigator.clipboard.writeText(link)
        }
        catch (e) {
            
        }
    }
     
</script>



<style lang="scss">
    .unload {
        position: relative;
        display: grid;
        grid-template-columns: 35% 1fr;
        grid-template-rows: auto auto;
        grid-template-areas: "list1 info"
                            "list2 info";
        @include active-by ("xslg") {
            grid-template-columns: 1fr;
            grid-template-rows: auto auto auto;
            grid-template-areas: "list1" 
                                 "info"
                                 "list2" ;
        }
        gap: 15px;
        font-size: 14px;
        line-height: 1.2;
        &__card {
            grid-area: list1;
        }
        &__list {
            grid-area: list2;
        }
        &__info {
            grid-area: info;
            position: relative;
            flex-grow: 1;
            align-self: stretch;
            width: 100%;
            @include active-by("xslg") {
                position: sticky;
                top: 15px
            }
            .info {
                &__block {
                    position: sticky;
                    top: 15px;
                    text-align: center;
                    padding: 15px;
                }
                &__linkBlock {
                    padding: 15px;
                    position: sticky;
                    top: 15px;
                    .linkBlock {
                        &__button {
                            margin-left: 15px;
                        }
                    }
                }
                &__head {
                    font-size: 16px;
                }
            }  
        }
        &__card {
            .body {
                &__list {
                    padding-left: 0;
                    margin: 0;
                    list-style-position: outside;
                    list-style-type: "-";
                }
            }
        } 
    }

</style>

