<template>
    <div class="container mb-3">
        <form action="">
            <div class="mb-3">
                <label for="" class="form-label">
                    Page Title
                </label>
                <input type="text"
                class="form-control"
                v-model="pageTitle"
                />
            </div>
            <div class="mb-3">
                <label for="" class="form-label">
                    Content
                </label>
                <textarea
                type="text"
                class="form-control"
                rows="5" 
                v-model="content"></textarea>
            </div>
            <div class="mb-3">
                <label for="image" class="form-label">
                    Upload Image
                </label>
                <input type="file"
                        id="media"
                        accept="image/*"
                       class="form-control"
                       @change="handleImageUpload"
                />
            </div>
            <div class="mb-3">
                <button
                class="btn btn-primary"
                :disabled="isFormInvalid"
                @click.prevent="pageCreated({pageTitle, content})">
                Add Product
                </button>
            </div>
        </form>
        <div v-if="imagePreview" class="mb-3">
            <h5>Image Preview:</h5>
            <img :src="imagePreview" alt="Image preview" class="img-fluid" />
        </div>
    </div>
</template>

<script>
export default {
    props: ['pageCreated'],
    computed: {
        isFormInvalid() {
            return !this.pageTitle || !this.content || !this.image;
        }
    },
    data() {
        return {
            pageTitle: '',
            content: '',
            image: null,
            imagePreview: null
        }
    },
    methods: {
        submitForm() {
            if(!this.pageTitle || !this.content || !this.image)
            {
                alert('Please fill the form.')
                return;
            }

            this.pageCreated({
                pageTitle: this.pageTitle,
                content: this.content,
                image: this.image
            })
        },
        handleImageUpload(event) {
            const file = event.target.files[0];
            if (file && file.type.startsWith('image/')) {
                this.image = file; // Store the selected image file
                this.imagePreview = URL.createObjectURL(file); // Create a preview URL for the image
            } else {
                alert('Please select a valid image file.');
            }
        }
    }
}
</script>

<style :scoped>
img.img-fluid {
    max-width: 200px;
    margin-top: 10px;
} 
</style>