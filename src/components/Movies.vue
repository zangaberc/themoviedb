<template>
    <div>
        <div class="flex-container">
            <div class="filters">
                <h3 class="style-genres-h3">
                    Genres
                </h3>
                <div class="btn-group">
                    <button type="button" class="pill-button"  v-for="item in this.genres" @click="filter(item.id)">
                        {{item.name}}
                    </button>
                    <div class="pill-button-clear-padding-top" v-if="clearGenresStatus">
                        <button type="button" class="pill-button-clear"  @click="clearGenres">
                            Clear
                        </button>
                    </div>
                </div>
            </div>
            <div class="movies">
                <div class="card-container">
                    <div class="card" v-if="typeof this.posts !== 'undefined' && this.posts.length > 0" v-for="item in this.posts">
                        <button class="button-info"></button>
                        <img :src="'https://image.tmdb.org/t/p/w500/'+item.poster_path">
                        <div class="card-content">
                            <p class="style-movie-h">
                                {{ item.original_title }}
                            </p>
                            <p class="style-date">
                            {{ item.newDateFormat }}
                            </p>
                        </div>
                    </div>
                    <div v-else>
                        <div style="top: 50%; left: 50%;">
                            The selected genre does not find a match.
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import Axios from 'axios'

export default {
    data () {
        return {
            posts: [],
            genres: [],
            newMovies: [],
            movieData: [],
            clearGenresStatus: false
        }
    },
    mounted () {
        this.getfirstMovies()
        this.getGenres()
    },
    methods: {
        getfirstMovies() {
            Axios.get('https://api.themoviedb.org/3/discover/movie?api_key=28089ccc7a3e0fe408d8ec4b4fadbbdb')
            .then(response => {
                this.movieData = response.data.results
                for (let i = 0; i < this.movieData.length; i++) {
                    const date = new Date(this.movieData[i].release_date),
                    newDateFormat = date.toLocaleDateString('en-US', {year: 'numeric', month: 'short', day: 'numeric'})
                    this.movieData[i].newDateFormat = newDateFormat
                    this.posts.push(this.movieData[i])
                }
            })
            .catch(err => {
                alert('ERROR:' + err)
            });
        },
        getGenres() {
            Axios.get('https://api.themoviedb.org/3/genre/tv/list?api_key=28089ccc7a3e0fe408d8ec4b4fadbbdb')
            .then(response => {
                for (let i = 0; i < response.data.genres.length; i++) {
                    this.genres.push(response.data.genres[i])
                }
            })
            .catch(err => {
                alert('ERROR:' + err)
            });
        },
        filter (item) {
            this.posts = []
            for (let i = 0; i < this.movieData.length; i++) {
                for (let j = 0; j < this.movieData[i].genre_ids.length; j++) {
                    if (this.movieData[i].genre_ids[j] === item) {
                        this.posts.push(this.movieData[i])
                    }
                }
            }
            this.clearGenresStatus = true
        },
        clearGenres () {
            this.posts = this.movieData
            this.clearGenresStatus = false
        }
    }
}
</script>
<style scoped>
.style-genres-h3 {
    font-size: 1em;
    font-weight: 300;
    padding: 20px 20px 0px 20px;
}

.btn-group {
    padding: 10px;
}

.close-btn {
    font-size: 60px;
    font-weight: bold;
    color: #000;
}

.flex-container {
    display: flex;
    flex-wrap: wrap;
}

.flex-container > div.filters{
    width: 200px;
    margin: 10px;
}

.flex-container > div.movies{
    width: 1200px;
    min-width: 1200px;
    margin: 10px;
}

.filters {
    border: 1px solid #e3e3e3;
    min-width: 260px;
    width: 260px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, .1);
    padding: 1px;
}

.pill-button-clear-padding-top {
    padding-top: 20px;
}

.pill-button-clear {
    font-size: 12px;
    padding: 0.5em 1em;
    margin: 0.25em;
    border-radius: 1em;
    border: none;
    outline: none;
    cursor: pointer;
    color: black;
    background-color: white;
    border: 1px solid #9e9e9e;
    width: 100%;
}

.pill-button-clear:hover {
    background: red;
    border: 1px solid red;
    color: white;
}

.pill-button {
    font-size: 12px;
    padding: 0.5em 1em;
    margin: 0.25em;
    border-radius: 1em;
    border: none;
    outline: none;
    cursor: pointer;
    color: black;
    background-color: white;
    border: 1px solid #9e9e9e;
}

.pill-button:hover {
    background: #01b4e4;
    border: 1px solid #01b4e4;
    color: white;
}

.card-container {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
}

.card {
    width: 185px;
    height: 390px;
    background-color: white;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0px 2px 4px rgba(0,0,0,0.2);
    margin: 20px;
    position: relative;
}

.card img {
    width: 100%;
    height: auto;
}

.style-date {
    font-size: 1em;
    margin: 0;
    padding: 0;
    color: rgba(0, 0, 0, .6);
}

.style-movie-h {
    font-weight: 700;
    color: #000;
}

.card-content {
    padding: 26px 10px 12px;
}

.card-content p {
    font-size: 1em;
    margin: 0;
    padding: 0;
    color: rgba(0, 0, 0, .6);
}

.button-info {
    position: absolute;
    right: 1em;
    top: 1em;
    display: block;
    width: 20px;
    height: 20px;
    border:2px solid #d3d3d3;
    border-radius:50%;
    background-color:rgba(211,211,211);
    color:#d3d3d3;
    opacity: 0.5;
}

.button-info:hover {
    background-color: #01b4e4;
    color: white;
    opacity: 5;
    border:2px solid #01b4e4;
}
</style>
