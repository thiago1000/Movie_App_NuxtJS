<template>
  <!-- Carregamento -->
  <Loading v-if="$fetchState.pending" />

  <!-- Informações -->
    <div v-else class="single-movie container">
        <NuxtLink class="button" :to="{ name: 'index' }"> Voltar </NuxtLink>
        <div class="movie-info">
            <div class="movie-img">
                <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt=""/>
            </div>
            <div class="movie-content">
                <h1>Titulo: {{ movie.title }}</h1>
                <p class="movie-fact tagline">
                    <span>Slogan:</span> "{{ movie.tagline }}"
                </p>
                <p class="movie-fact">
                    <span>Lançamento:</span>
                    {{
                        new Date(movie.release_date).toLocaleString('pt-br', {
                            month: 'long',
                            day: 'numeric',
                            year: 'numeric',
                        })
                    }}
                </p>
                <p class="movie-fact">
                    <span>Duração:</span> {{ movie.runtime }} minutos
                </p>
                <p class="movie-fact">
                    <span>Arrecadação:</span>
                    {{
                        movie.revenue.toLocaleString('pt-br', {
                            style: 'currency',
                            currency: 'BRL',
                        })
                    }}
                </p>
                <p class="movie-fact"><span>Sinopse:</span> {{ movie.overview }}</p>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import Loading from '~/components/Loading'

export default {
    name: 'singleMovie',

    components: {
		Loading,
	},

    async fetch() {
        await this.getSingleMovie();
    },
    fetchDelay: 1000,

    head() {
        return {
            title: this.movie.title,
        }
    },

    data() {
        return {
            movie: '',

            trailers: '',
        }
    },

    methods: {
        async getSingleMovie() {
            const data = axios.get(
                `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=1de8edc527f095ee0909db33e2ac39ff&language=pt-BR`
            )
            const result = await data
            this.movie = result.data
        },
    },
}
</script>

<style lang="scss">
    .single-movie {
        color: #fff;
        min-height: 100vh;
        display: flex;
        flex-direction: column;
        justify-content: center;
        padding: 32px 16px;
        .button {
            align-self: flex-start;
            margin-bottom: 32px;
        }
        .movie-info {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 32px;
            color: #fff;
            @media (min-width: 800px) {
                flex-direction: row;
                align-items: flex-start;
            }
            .movie-img {
                img {
                    max-height: 500px;
                    width: 100%;
                    @media (min-width: 800px) {
                        max-height: 700px;
                        width: initial;
                    }
                }
            }
            .movie-content {
                h1 {
                    font-size: 56px;
                    font-weight: 400;
                }
                .movie-fact {
                    margin-top: 12px;
                    font-size: 20px;
                    line-height: 1.5;
                    span {
                        font-weight: 600;
                        text-decoration: underline;
                    }
                }
                .tagline {
                    font-style: italic;
                    span {
                        font-style: normal;
                    }
                }
            }
        }
    }
</style>