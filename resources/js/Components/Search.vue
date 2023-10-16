<script setup>
import { autocomplete } from '@algolia/autocomplete-js'
import { onMounted } from 'vue'
import { meilisearchAutocompleteClient, getMeilisearchResults } from '@meilisearch/autocomplete-client'

const client = new meilisearchAutocompleteClient({
    url: 'http://localhost:7700',
    apiKey: 'masterKey'
})

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
                        return getMeilisearchResults({
                            searchClient: client,
                            queries: [
                                {
                                    indexName: 'users',
                                    query: query
                                }
                            ]
                        })
                    },
                    getItemUrl({ item }) {
                        return item.url
                    },
                    templates: {
                        header ({ html }) {
                            return html`
                                <span className="aa-SourceHeaderTitle">Users</span>
                                <div className="aa-SourceHeaderLine"></div>
                            `
                        },
                        item ({ item, components, html }) {
                            return html`
                                <a href="${item.url}" class="flex items-center space-x-2">
                                    <img src="${item.avatar_url}" class="w-8 h-8 rounded-full" />
                                    <span>
                                        ${components.Highlight({
                                            hit: item,
                                            attribute: 'name',
                                            tagName: 'em'
                                        })}
                                    </span>
                                </a>
                            `
                        }
                    }
                },
                {
                    sourceId: 'courses',
                    getItems ({ query }) {
                        return getMeilisearchResults({
                            searchClient: client,
                            queries: [
                                {
                                    indexName: 'courses',
                                    query: query
                                }
                            ]
                        })
                    },
                    getItemUrl({ item }) {
                        return item.url
                    },
                    templates: {
                        header ({ html }) {
                            return html`
                                <span className="aa-SourceHeaderTitle">Courses</span>
                                <div className="aa-SourceHeaderLine"></div>
                            `
                        },
                        item ({ item, components, html }) {
                            return html`
                                <a href="${item.url}" class="flex items-center space-x-2">
                                    <span>
                                        ${components.Highlight({
                                            hit: item,
                                            attribute: 'title',
                                            tagName: 'em'
                                        })}
                                    </span>
                                </a>
                            `
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
