<template>
    <a-modal v-if="props.routeName === 'regions'" v-model:open="props.open" title="Добавить новый регион" @ok="saveData" closable @cancel="toggleModal">
        <a-form
            :model="newItem"
            name="basic"
            autocomplete="off"
            @finish="onFinish"
            @finishFailed="onFinishFailed"
        >
            <a-form-item
                label="Регион"
                name="Регион"
            >
                <a-input v-model:value="newItem.data.NAME" />
            </a-form-item>
            <a-form-item
                label="Тип"
                name="Тип" 
            >
                <a-select v-model:value="newItem.data.PROPERTY_REGION_TYPE">
                    <a-select-option value="3479">Область</a-select-option>
                    <a-select-option value="3478">Город</a-select-option>
                </a-select>
            </a-form-item>
        </a-form>
        <template #footer>
            <a-button key="back" @click="toggleModal">Отменить</a-button>
            <a-button key="submit" type="primary" :loading="loading" @click="saveData">Сохранить</a-button>
        </template>
    </a-modal>
    <a-modal v-else-if="props.routeName === 'vicinity'" v-model:open="props.open" title="Добавить новый округ" @ok="saveData" closable @cancel="toggleModal">
        <a-form
            :model="newItem"
            name="basic"
            autocomplete="off"
            @finish="onFinish"
            @finishFailed="onFinishFailed"
        >
            <a-form-item
                label="Округ"
                name="Округ"
            >
                <a-input v-model:value="newItem.data.NAME" />
            </a-form-item>
            
            <a-form-item
                label="Регион"
                name="Регион" 
            >
                <a-select v-model:value="newItem.data.PROPERTY_REGION_ID">
                    <a-select-option v-for="region in regions" :key="region.id" :value="region.id">{{ region.value }}</a-select-option>
                </a-select>
            </a-form-item>
        </a-form>
        <template #footer>
            <a-button key="back" @click="toggleModal">Отменить</a-button>
            <a-button key="submit" type="primary" :loading="loading" @click="saveData">Сохранить</a-button>
        </template>
    </a-modal>
    <a-modal v-else-if="props.routeName === 'highway'" v-model:open="props.open" title="Добавить новый шоссе" @ok="saveData" closable @cancel="toggleModal">
        <a-form
            :model="newItem"
            name="basic"
            autocomplete="off"
        >
            <a-form-item
                label="Название шоссе"
                name="Название шоссе"
            >
                <a-input v-model:value="newItem.data.NAME" />
            </a-form-item>
            
            <a-form-item
                label="В склонении"
                name="В склонении" 
            >
                <a-input v-model:value="newItem.data.PROPERTY_FOR_TITLE" />
            </a-form-item>
        </a-form>
        <template #footer>
            <a-button key="back" @click="toggleModal">Отменить</a-button>
            <a-button key="submit" type="primary" :loading="loading" @click="saveData">Сохранить</a-button>
        </template>
    </a-modal>
    <a-modal v-else-if="props.routeName === 'place'" v-model:open="props.open" title="Добавить новый населённый пункт" @ok="saveData" closable @cancel="toggleModal">
        <a-form
            :model="newItem"
            name="basic"
            autocomplete="off"
        >
            <a-form-item
                label="Населённый пункт"
                name="Населённый пункт"
            >
                <a-input v-model:value="newItem.data.NAME" />
            </a-form-item>
            <a-form-item
                label="Тип"
                name="Тип" 
            >
                <a-select v-model:value="newItem.data.PROPERTY_LOCALITY_TYPE">
                    <a-select-option v-for="locality in localityItems" :key="locality.id" :value="locality.id">{{ locality.value }}</a-select-option>
                </a-select>
            </a-form-item>
            <a-form-item
                label="Район"
                name="Район" 
            >
                <a-select v-model:value="newItem.data.PROPERTY_DISTRICT_ID">
                    <a-select-option v-for="district in districts" :key="district.id" :value="district.id">{{ district.value }}</a-select-option>
                </a-select>
            </a-form-item>
            <a-form-item
                label="Округ"
                name="Округ" 
            >
                <a-select v-model:value="newItem.data.PROPERTY_VICINITY_ID">
                    <a-select-option v-for="vicinity in vicinities" :key="vicinity.id" :value="vicinity.id">{{ vicinity.value }}</a-select-option>
                </a-select>
            </a-form-item>
        </a-form>
        <template #footer>
            <a-button key="back" @click="toggleModal">Отменить</a-button>
            <a-button key="submit" type="primary" :loading="loading" @click="saveData">Сохранить</a-button>
        </template>
    </a-modal>
    <a-modal v-else-if="props.routeName === 'districts'" v-model:open="props.open" title="Добавить новый район" @ok="saveData" closable @cancel="toggleModal">
        <a-form
            :model="newItem"
            name="basic"
            autocomplete="off"
        >
            <a-form-item
                label="Район"
                name="Район"
            >
                <a-input v-model:value="newItem.data.NAME" />
            </a-form-item>
            <a-form-item
                label="Округ"
                name="Округ" 
            >
                <a-select v-model:value="newItem.data.PROPERTY_VICINITY_ID">
                    <a-select-option v-for="vicinity in vicinities" :key="vicinity.id" :value="vicinity.id">{{ vicinity.value }}</a-select-option>
                </a-select>
            </a-form-item>
            <a-form-item
                label="Регион"
                name="Регион" 
            >
                <a-select v-model:value="newItem.data.PROPERTY_REGION_ID">
                    <a-select-option v-for="region in regions" :key="region.id" :value="region.id">{{ region.value }}</a-select-option>
                </a-select>
            </a-form-item>
            <a-form-item
                label="Популярный район"
                name="Популярный район" 
            >
                <a-select v-model:value="newItem.data.PROPERTY_POPULAR_DISTRICT">
                    <a-select-option value="1">Да</a-select-option>
                </a-select>
            </a-form-item>
        </a-form>
        <template #footer>
            <a-button key="back" @click="toggleModal">Отменить</a-button>
            <a-button key="submit" type="primary" :loading="loading" @click="saveData">Сохранить</a-button>
        </template>
    </a-modal>
    <a-modal v-else-if="props.routeName === 'metro'" v-model:open="props.open" title="Добавить новый метро" @ok="saveData" closable @cancel="toggleModal">
        <a-form
            :model="newItem"
            name="basic"
            autocomplete="off"
        >
            <a-form-item
                label="Метро"
                name="Метро"
            >
                <a-input v-model:value="newItem.data.NAME" />
            </a-form-item>
            <a-form-item
                label="Линия метро"
                name="Линия метро" 
            >
                <a-select v-model:value="newItem.data.PROPERTY_METRO_LINE">
                    <a-select-option v-for="line in metrolines" :key="line.id" :value="line.id">{{ line.value }}</a-select-option>
                </a-select>
            </a-form-item>
            <a-form-item
                label="Циан ID"
                name="Циан ID" 
            >
                <a-input v-model:value="newItem.data.PROPERTY_CIAN_ID" />
            </a-form-item>
        </a-form>
        <template #footer>
            <a-button key="back" @click="toggleModal">Отменить</a-button>
            <a-button key="submit" type="primary" :loading="loading" @click="saveData">Сохранить</a-button>
        </template>
    </a-modal>
    <a-modal v-else-if="props.routeName === 'metro_line'" v-model:open="props.open" title="Добавить новый линию метро" @ok="saveData" closable @cancel="toggleModal">
        <a-form
            :model="newItem"
            name="basic"
            autocomplete="off"
        >
            <a-form-item
                label="Линия метро"
                name="Линия метро"
            >
                <a-input v-model:value="newItem.data.NAME" />
            </a-form-item>
        </a-form>
        <template #footer>
            <a-button key="back" @click="toggleModal">Отменить</a-button>
            <a-button key="submit" type="primary" :loading="loading" @click="saveData">Сохранить</a-button>
        </template>
    </a-modal>
    <a-modal v-else-if="props.routeName === 'village'" v-model:open="props.open" title="Добавить новый коттеджый поселек" @ok="saveData" closable @cancel="toggleModal">
        <a-form
            :model="newItem"
            name="basic"
            autocomplete="off"
        >
            <a-form-item
                label="Название"
                name="Название"
            >
                <!-- v-model value we need to add newItem object too -->
                <a-input v-model:value="newItem.data.NAME" />
            </a-form-item>
            <a-form-item
                label="ССЫЛКА НА ЦИАН"
                name="ССЫЛКА НА ЦИАН"
            >
                <a-input v-model:value="newItem.data.PROPERTY_CIAN_LINK" />
            </a-form-item>
            
        </a-form>
        <template #footer>
            <a-button key="back" @click="toggleModal">Отменить</a-button>
            <a-button key="submit" type="primary" :loading="loading" @click="saveData">Сохранить</a-button>
        </template>
    </a-modal>
</template>
<script setup>
    import { reactive, ref, onMounted, computed } from 'vue';
    import { useUserStore } from '../stores/user.module.js';
    
    const myStore = useUserStore();
    const loading = ref(false);

    const regions = computed(() => {
        return myStore.regions;
    })
    const districts = computed(() => {
        return myStore.districts;
    })
    const vicinities = computed(() => {
        return myStore.vicinity;
    })
    const metrolines = computed(() => {
        return myStore.metro_line;
    })


    const props = defineProps({
        routeName: String,
        open: Boolean
    })

    const emits = defineEmits(['toggle'], ['save'])   

    // main object for sending data to backend
    const newItem = reactive({
        type: 'new',
        data: {
            //for all
            'NAME': '',
            //region
            'PROPERTY_REGION_TYPE': '',
            //vicinity
            'PROPERTY_REGION_ID': '',
            //highway
            'PROPERTY_FOR_TITLE': '',
            //locality
            'PROPERTY_LOCALITY_TYPE': '',
            'PROPERTY_DISTRICT_ID': '',
            'PROPERTY_VICINITY_ID': '',
            //districts
            "PROPERTY_POPULAR_DISTRICT": '',
            //metro
            "PROPERTY_CIAN_ID": '',
            "PROPERTY_METRO_LINE": '',
            // village
            "PROPERTY_CIAN_LINK" : ''

        },
        place: props.routeName
    })

    const localityItems = ref([
        {
            id: 3492, 
            value: 'сельское поселение'
        },
        {
            id: 3397, 
            value: 'село'
        },
        {
            id: 3396, 
            value: 'деревня'
        },
        {
            id: 3600, 
            value: 'город'
        },
        {
            id: 3395, 
            value: 'поселок'
        },
    ])

    const toggleModal = () => {
        emits('toggle');
    };

    // main function for sending data to backend
    const saveData = async (e) => {
        loading.value = true;
        emits('save');
        await myStore.addNewPlaceChild(newItem);
        await myStore.getPlacesChild(props.routeName);
        loading.value = false;
        // Clear the data properties
        Object.keys(newItem.data).forEach((key) => {
            newItem.data[key] = '';
        });
    };

    onMounted(() => {
        fetchSubChildData();
    })

    const fetchSubChildData = async () => {
        try {
            await myStore.getPlacesChild('regions')
            await myStore.getPlacesChild('districts')
            await myStore.getPlacesChild('vicinity')
            await myStore.getPlacesChild('metro_line')
        } catch (error) {
            console.error('Error fetching data in component:', error);
        }
    }
</script>