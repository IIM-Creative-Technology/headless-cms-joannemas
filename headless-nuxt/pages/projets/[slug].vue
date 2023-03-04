<template>
    <div v-if="projet">
        <div class="projet-container">
            <div class="projet-image" v-if="projet.image">
                <img :src="projet.image.url" alt="" width="500">
            </div>
            <div class="projet-description">
                <h2>{{ projet.name }}</h2>
                <p>{{ projet.description }}</p>
                <p v-if="projet.technologies.length">Technologies utilisées :</p>
                <ul>
                    <li v-for="technologie in projet.technologies">
                        {{ technologie.name }}
                    </li>
                </ul>
                <div v-if="projet.lien" class="link_button">
                    <a :href="projet.lien" target="_blank">Voir le projet</a>
                </div>

                <div>
                    <nuxt-link to="/" class="return_button">
                        <p>Retour</p>
                    </nuxt-link>
                </div>

                <div class="buttons_pages">
                    <div v-if="previousproject">
                        <nuxt-link :to="`/projets/${previousproject.slug}`" class="previous_button">
                            <p>Projet précédent</p>
                        </nuxt-link>
                    </div>
                    <div v-if="nextproject">
                        <nuxt-link :to="`/projets/${nextproject.slug}`" class="next_button">
                            <p>Projet suivant</p>
                        </nuxt-link>
                    </div>
                </div>

            </div>
            
        </div>

    </div>
</template>

<script setup>

const { findOne } = useStrapi()
const route = useRoute()
const projet = ref()


onMounted(async () => {
    projet.value = await findOne(`projets?filters[slug]=${route.params.slug}&populate=deep`)
    projet.value = projet.value.data[0]
})

const { find } = useStrapi()
const nextproject = ref()
const previousproject = ref()


onMounted(async () => {
    const currentSlug = route.params.slug
    const projects = await find(`projets?populate=deep`)
    const currentIndex = projects.data.findIndex(project => project.slug === currentSlug)
    nextproject.value = projects.data[currentIndex + 1] || null
    previousproject.value = projects.data[currentIndex - 1] || null
})


</script>

<style scoped>

    .projet-container {
        display: flex;
        flex-direction: row;
        align-items: center;
    }

    .projet-image {
        margin: 0 100px;
    }

    .projet-image img {
        max-height: 800px;
        object-fit: cover;
        object-position: top;

    }

    .projet-description {
        margin-right: 30px;
        flex-direction: column;
    }

    .projet-description h2 {
        font-size: 2em;
        margin-bottom: 10px;
        font-family: Impact;
        letter-spacing: 3px;
    }

    .projet-description p {
        font-size: 1.2em;
        line-height: 1.5em;
        margin-bottom: 20px;
    }
    .projet-description ul {
        list-style-type: none;
        font-size: 1em;
        line-height: 1.5em;
    }

    .projet-description ul li::before {
        content: "→";
        margin-right: 5px;
    }

    .return_button {
        margin-top: 20px;
        text-decoration: none;
    }

    .return_button p {
        background-color: #007bff;
        padding: 10px;
        border-radius: 5px;
        text-decoration: none;
        width: max-content;
        color: #ffffff;
    }
    .link_button a{
        background-color: #f27df8;
        padding: 10px;
        border-radius: 5px;
        text-decoration: none;
        width: max-content;
        color: #ffffff;
        font-size: 1.2em;
    }

    .buttons_pages {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        margin-top: 20px;
    }

    .previous_button p{
        background-color: #28dc94;
        padding: 10px;
        border-radius: 5px;
        text-decoration: none;
        width: max-content;
        color: #ffffff;
    }

    .next_button p{
        background-color: #ff0073;
        padding: 10px;
        border-radius: 5px;
        text-decoration: none;
        width: max-content;
        color: #ffffff;
    }

    .previous_button, .next_button {
        text-decoration: none;
    }


</style>