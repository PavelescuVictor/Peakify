<template>
    <div class="gallery__item">
        <img :src="imgSrc" @click="showFullImage()" />
        <div class="gallery__item--fullScreen" v-bind:class="{ active : isActive }">
            <img :src="imgSrc" @click="showFullImage()" />
        </div>
    </div>
</template>

<script>
import axios from "axios";
export default {
    name: "GalleryItem",
    props: ["imageLink", "itemActive"],
    methods: {
        showFullImage() {
            if (this.itemActive === false) {
                console.log(this.itemActive);
                this.isActive = !this.isActive;
                this.$emit("activeGalleryItem");
            } else if (this.isActive === true) {
                this.isActive = !this.isActive;
                this.$emit("activeGalleryItem");
            }
        },
    },
    data() {
        return {
            isActive: false,
            imgSrc: null,
        };
    },
    created() {
        axios.get(this.imageLink).then((response) => {
            this.imgSrc = response.data[1];
        });
    },
};
</script>

<style scoped>
.gallery__item {
    height: 100%;
    width: 100%;
    overflow: hidden;
}

.gallery__item img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.gallery__item--fullScreen {
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    position: fixed;
    display: none;
    z-index: 2;
}

.active {
    display: block;
}

.inactive {
    display: none;
}
</style>