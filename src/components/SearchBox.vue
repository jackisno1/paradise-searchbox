<script setup>
import { ref } from 'vue';

async function get(url, options = {}) {
    const response = await fetch(url, options);
    if (response.ok) {
        return response.json();
    } else {
        throw new Error('Cant get resource');
    }
}

let resource = [];

await get('https://restcountries.com/v3.1/all?fields=name').then(data => {
    resource = data.map(item => {
        return {
            value: item.name.common,
            text: item.name.common
        }
    }).sort((a, b) => { if (a.value > b.value) return 1; else if (a.value < b.value) return -1; return 0 });
}).catch(e => {
    console.log(e);
})

const filterResource = ref([]);


const input = ref(null);
const preventFilter = ref(false);
defineProps({
    placeholder: {
        type: String,
        default: 'Type the search here...'
    },
    noResultText: {
        type: String,
        default: "No result."
    }
});
function filterDropdown() {
    _preventFilter(false);
    if (input.value) {
        let input = input.value.toLowerCase();
        filterResource.value = resource.filter(item => _blurSearch(item.value.toLowerCase(), input))
    } else {
        filterResource.value = [];
    }
}

function doAutoComplete(val) {
    _preventFilter(true);
    input.value = val;
    filterResource.value = [];
}

function _preventFilter(state) {
    preventFilter.value = state;
}

function _blurSearch(src, compare) {
    for (let i = 0; i < compare.length; i++) {
        let pos = src.indexOf(compare[i]);
        if (pos === -1) {
            return false;
        } else {
            src = src.slice(pos);
        }
    }
    return true;
}

</script>
<template>
    <div class="wrapper">
        <input type="text" v-model="input" :placeholder="placeholder" @keyup="filterDropdown">
        <div class="dropdown">
            <p v-for="item in filterResource" :key="item.id" class="dropdown-item" @click="doAutoComplete(item.value)">
                {{ item.text }}
            </p>
            <p v-if="!preventFilter && input && filterResource.length === 0"> {{ noResultText }} </p>
        </div>
    </div>
</template>
<style scoped>
.wrapper {
    width: 300px;
    position: relative;
}

.wrapper input {
    border-radius: 3px;
    font-size: 14px;
    width: 100%;
    padding: 4px;
}

.wrapper .dropdown {
    width: 100%;
    overflow-y: scroll;
    position: absolute;
    max-height: calc(100vh/4);
}

p {
    border-radius: 3px;
    font-size: 14px;
    margin: 2px;
    padding: 4px;
    background-color: #eee;
}

P.dropdown-item:hover {
    background-color: #ccc;
}
</style>
