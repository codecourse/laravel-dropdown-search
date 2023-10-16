<script setup>
import { autocomplete } from '@algolia/autocomplete-js'
import { onMounted } from 'vue'

onMounted(() => {
    autocomplete({
        container: '#search',
        placeholder: 'What are you looking for?',
        autoFocus: true,
        getSources () {
            return [
                {
                    sourceId: 'users',
                    getItems ({ query }) {
                        const items = [
                            { id: 1, name: 'Alex' },
                            { id: 2, name: 'Mabel' },
                        ]

                        return items.filter(({ name }) => name.toLowerCase().includes(query.toLowerCase()))
                    },
                    templates: {
                        item ({ item }) {
                            return item.name
                        }
                    }
                }
            ]
        }
    })
})

</script>

<template>
    <div id="search"></div>
</template>
