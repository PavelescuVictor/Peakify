<template>
    <div class="gallery">
        <div class="gallery__content">
            <p>What are you searching for?</p>
            <form @submit.prevent="getContent()">
                <input type="text" placeholder="Type here the topic" v-model="topic" />
            </form>
            <div class="gallery__items">
                <GalleryItem
                    v-for="image in images"
                    :key="image.data[0].nasa_id"
                    :imageLink="image.href"
                    @activeGalleryItem="activeGalleryItem"
                    :itemActive="itemActive"
                />
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";
import GalleryItem from "./GalleryItem.vue";
export default {
    name: "Gallery",
    data() {
        return {
            topic: "",
            images: [],
            itemActive: false,
        };
    },
    components: {
        GalleryItem,
    },
    methods: {
        activeGalleryItem() {
            this.itemActive = !this.itemActive;
        },
        getContent() {
            /*
            let date = new Date();
            let currentDay = date.getDate();
            let currentMonth = date.getMonth() - 1;
            let currentYear = date.getFullYear();
            axios.get(`https://api.nasa.gov/mars-photos/api/v1/rovers/${ query }/photos?api_key=DEMO_KEY&earth_date=${currentYear}-${currentMonth}-${currentDay}`).then( response => {
                this.images = response.data.photos;
                console.log(response);
            });
            console.log(`https://api.nasa.gov/mars-photos/api/v1/rovers/${ query }/photos?api_key=DEMO_KEY&earth_date=${currentYear}-${currentMonth}-${currentDay}`);
            */
            axios
                .get(
                    `https://images-api.nasa.gov/search?title=${this.topic}&media_type=image`
                )
                .then((response) => {
                    this.images = response.data.collection.items;
                    console.log(response.data.collection.items);
                    //console.log(response.data.collection.items[0].data[0].nasa_id);
                    //console.log(response.data.collection.items[0].href);
                });
        },
    },
};
</script>

<style scooped>
.gallery {
    height: 100%;
    width: 90%;
    margin: auto;
    background: #fff;
    display: flex;
    justify-content: center;
    border-radius: 15px;
    overflow: hidden;
}

.gallery__content {
    width: 90vw;
    height: 100%;
    background: #fff;
}

p {
    padding: 2rem;
    font-size: 2rem;
}

.gallery__content form {
    padding: 1rem;
}

.gallery__content form input {
    border: 0px;
    height: 35px;
    border-style: none;
    border-radius: 50px;
    padding: 1rem;
    font-size: 1.2rem;
    background: #eee;
}

.gallery__content form input:focus {
    outline: none;
}

.gallery__items {
    width: 100%;
    height: 100%;
    box-sizing: border-box;
    padding: 0.8rem 0.8rem;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    grid-gap: 0.2rem;
    justify-content: center;
    align-items: center;
    object-fit: cover;
}
</style>
