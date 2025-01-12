<template>
    <div class="container mb-3">
        <form action="">
            <div class="mb-3">
                <label for="" class="form-label">
                    Input search value
                </label>
                <input type="text"
                class="form-control"
                v-model="searchKey"
                />
            </div>
            <div class="row mb-3">
                <div class="form-check">
                    <input class="form-check-input" 
                    v-model="contentToo"
                    type="checkbox"/>
                    <label class="form-check-label" for="gridCheck1">
                    Also search in content</label>
                </div>

            </div>
            <div class="mb-3">
                <button
                class="btn btn-primary"
                :disabled="isSearchInvalid"
                @click.prevent="searchForPage({searchKey, contentToo})">
                Find Product
                </button>
            </div>
        </form>
    </div>
</template>

<script>
export default {
    props: ['searchForPage'],
    computed: {
        isSearchInvalid() {
            return !this.searchKey;
        }
    },
    data() {
        return {
            searchKey: '',
            contentToo: false
        }
    },
    methods: {
        submitForm() {
            if(!this.searchKey)
            {
                alert('Please fill the form.')
                return;
            }

            this.searchForPage({
                searchKey: this.searchKey,
                contentToo: this.contentToo
            })
        }
    }
}
</script>