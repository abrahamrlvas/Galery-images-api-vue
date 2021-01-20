<template>
    <div>
        <h1 class="text-center text-3xl text-white my-10 font-bold uppercase">Buscador de Imágenes Pixabay</h1>
        <div class="buscador max-w-lg mx-auto " id="scroll">
            <label class="block text-white font-bold uppercase">Término de búsqueda.</label>
            <input id=" hola" v-model="search" type="text" class="outline-none text-color-black mt-3 rounded w-full p-2" placeholder="Término de búsqueda. Ejemplo: Café o Futbol" >
            <button @click="fetchImages" class="focus:outline-none mt-5 w-full py-2 bg-yellow-400 cursor-pointer font-bold uppercase hover:bg-yellow-500 rounded">Buscar Imágenes</button>
        </div>
        <div class="container mx-auto mt-10  flex flex-wrap insert" >
            <div class="w-1/2 md:w-1/3 lg:w-1/4 mb-4 p-3 cont" v-for="image of images" :key="image.id">
                <div class="bg-white ">
                    <img class="w-full" v-bind:src="image.largeImageURL" />
                    <div class="p-4">
                        <p class="card-text">{{image.likes}} Me Gusta</p>
                        <p class="card-text">{{image.views}} Vistas </p>
        
                        <a :href="image.largeImageURL" 
                        rel="noopener noreferrer" 
                        target="_blank" class="bg-blue-800 w-full p-1 block mt-5 rounded text-center font-bold uppercase hover:bg-blue-500 text-white">Ver Imagen</a>
                    </div>
                </div>
            </div>
        </div>
        <div v-if="images.length> 0" class="container mb-1 text-center flex flex-wrap justify-center mx-auto mb-8">
            <button class="text-black font-bold py-2 w-20 mr-10 bg-yellow-400 hover:bg-yellow-500 rounded focus:outline-none" @click="changePage(page-1)">Anterior</button>
            <button class="text-black font-bold py-2 w-20  bg-yellow-400 hover:bg-yellow-500 rounded focus:outline-none" @click="changePage(page+1)">Siguiente</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Form',
    data(){
        return{
            search: '',
            key: '19802414-4a5899156ddc39f45f1adec45',
            images: [],
            page: 1,
            pages: 1
        }
    },

    methods: {
        async fetchImages(){

            if (this.search.length <=0) {
                console.log('error');
                return
            }else{
            const url = `https://pixabay.com/api/?key=${this.key}&q=${this.search}&per_page=32&page=${this.page}`
            const res = await fetch(url)
            const result = await res.json()
            this.images = result.hits
            this.pages = result.totalHits
            }

        },
        changePage(page){
            this.page = (page<=0 || page > this.pages) ? this.page : page
            this.fetchImages()
            const element = document.getElementById('scroll');
            element.scrollIntoView();
        },
    }
}
</script>

<style scoped>
.cont img {
  height: 250px;
}
</style>>

</style>