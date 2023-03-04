<template>
    <div v-if="projets">
        <h1>Portfolio</h1>
        
        
        <!-- <nuxt-link :to="`/projets/${projet.slug}`" v-for="projet in projets.data">
            <div class="card">
                {{ projet.name }}
                <img :src="projet.image.url" alt="" width="50">
            </div>
        </nuxt-link> -->
        

        <div class="projets-container">
            <div v-for="type in types.data">
                <div class="type-project-banner">
                    <h2 class="type-project">{{ type.name }}</h2>
                </div>
                <div class="projets">
                    <nuxt-link :to="`/projets/${projet.slug}`" v-for="projet in type.projets">
                        <div class="card">
                            <p>{{ projet.name }}</p>
                            <div v-if="projet.image">
                                <img :src="projet.image.url" alt="" width="200">
                            </div>
                        </div>
                    </nuxt-link>
                </div>
            </div>
        </div>
        


        <div>
            <div class="filter-type-project-banner">
                <h3>Filtrer par type</h3>
            </div>
            <button :class="{ active: activeFilter === 'all' }" @click="filterProjets('all')">Tous</button>
            <button v-for="type in types.data" :class="{ active: activeFilter === type.name }" @click="filterProjets(type.name)">{{ type.name }}</button>

            <div class="projets">
                <nuxt-link :to="`/projets/${projet.slug}`" v-for="projet in filteredProjets" :key="projet.id">
                <div class="card">
                    <p>{{ projet.name }}</p>
                        <div v-if="projet.image">
                            <img :src="projet.image.url" alt="" width="200">
                        </div>
                </div>
                </nuxt-link>
            </div>
        </div>
        

    </div>




</template>

<script setup>

    const { find } = useStrapi()

    const projets = ref()
    const types = ref()
    const activeFilter = ref('all')
    const technologies = ref()
    const introduction = ref()

    const filteredProjets = computed(() => {
        if (activeFilter.value === 'all') {
            return projets.value.data
        } else {
            // return projets.value.data.filter(projet => projet.types.name === [activeFilter.value])
            return types.value.data.filter(type => type.name === activeFilter.value)[0].projets
        }
    })

    const filterProjets = (type) => {
        activeFilter.value = type
    }



    onMounted(async () => {
        projets.value = await find('projets', {populate: 'deep'})
        // console.log(projets.value);
        // types.value = new Set(projets.value.data.map(projet => projet.type))
    })

    onMounted(async () => {
        types.value = await find('types', {populate: 'deep'})
        console.log(introduction.value);
    })



</script>

<style scoped>

h1 {
    text-align: center;
    font-size: 40px;
    margin-bottom: 100px;
    font-family: Impact;
    letter-spacing: 3px;
}
.projets {
    display: flex;
    flex-wrap: wrap;
}

.projets a {
    text-decoration: none;
    color: #333;
}

.type-project {
    font-size: 24px;
}


.card {
    background-color: #fff;
    border: 1px solid #ddd;
    border-radius: 4px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    padding: 20px;
    margin-bottom: 10px;
    margin-right: 25px; 
}

.card img {
    margin-right: 10px;
}

button {
    background-color: #fff;
    border: 1px solid #ddd;
    border-radius: 4px;
    color: #333;
    cursor: pointer;
    margin: 10px 5px;
    padding: 5px 10px;
}

button:hover {
    background-color: #f5f5f5;
}

.active {
    background-color: #fd93c8;
    color: white;
}

.type-project-banner {
    background-color: #fd93c8;
    color: #fff;
    padding: 10px;
    margin: 25px 0;
    font-family: Verdana;
    border-radius: 5px;
    width: 40%;
}

.filter-type-project-banner {
    background-color: #fddf93;
    color: #fff;
    padding: 10px;
    margin: 25px 0;
    font-family: Verdana;
    border-radius: 5px;
    width: 60%;
    text-align: center;
}

h3 {
    font-size: 1.5rem;
    font-family: verdana;
}

.card img {
    height: 250px;
    object-fit: cover;
    object-position: top;
}

.card p {
    margin-top: 10px;
    text-decoration: none;
    font-size: 1rem;
    font-weight: 600;
    font-family: verdana;
    text-align: center;
    color: #000;
}


</style>