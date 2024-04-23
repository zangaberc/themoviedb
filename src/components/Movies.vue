<template>
        <div>
            <h2 class="title">
                Popular movies
            </h2>
        </div>
        <div>
            <div class="float-first">
                <div class="green">
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
                </div>
            </div>

            <div class="float-child">
                <div class="blue"><div class="movies">
                    <div class="card-container">
                        <div class="card" v-if="typeof this.posts !== 'undefined' && this.posts.length > 0" v-for="item in this.posts">
                            <button class="button-info"></button>
                            <img :src="'https://image.tmdb.org/t/p/w500/'+item.poster_path">
                            <div class="containerProgress">
                                <div class="progress" style="--i:85;--clr:#43f94a;">
                                    <h3>
                                        85<span>%</span>
                                    </h3>
                                </div>
                            </div>
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
                            <div class="no-match">
                                The selected genre does not find a match.
                            </div>
                        </div>
                        <button class="button-add-more" @click="loadMoreMovies" v-if="!clearGenresStatus">
                        Load More
                        </button>
                    </div>
                </div>
            </div>
        </div>
        <div style="clear: both;"/>
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
            clearGenresStatus: false,
            numberLoadedPages: 1,
            newPageData: []
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
                this.posts = []
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
        },
        loadMoreMovies () {
            this.numberLoadedPages++
            console.log(this.numberLoadedPages)
            Axios.get('https://api.themoviedb.org/3/discover/movie?api_key=28089ccc7a3e0fe408d8ec4b4fadbbdb&page=' + this.numberLoadedPages)
            .then(response => {
                this.newPageData = response.data.results
                for (let i = 0; i < this.newPageData.length; i++) {
                    const date = new Date(this.newPageData[i].release_date),
                    newDateFormat = date.toLocaleDateString('en-US', {year: 'numeric', month: 'short', day: 'numeric'})
                    this.newPageData[i].newDateFormat = newDateFormat
                    this.movieData.push(this.newPageData[i])
                    this.posts.push(this.newPageData[i])
                }
            })
            .catch(err => {
                alert('ERROR:' + err)
            });
        }
    }
}
</script>
<style scoped>
.float-first {
    float: left;
    padding-left: 100px;
    padding-top: 20px;

}

.float-child {
    width: 83%;
    padding: 20px;
} 

.button-add-more {
    border-radius: 12px;
    display: block;
    width: 91%;
    border: none;
    padding: 14px 28px;
    font-size: 16px;
    cursor: pointer;
    text-align: center;
    background-color: #01b4e4;
    color: white;
    font-size: 1.5em;
    font-weight: 700;
    margin-bottom: 30px
}

.button-add-more:hover {
    color: black;
}

.no-match {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    min-height: 100vh;
    color: lightgray;
}

.title {
    font-size: 1.6em;
    font-weight: 600;
    padding-left: 100px;
    padding-top: 30px;
}

.containerProgress {
    position: absolute;
    display:fixed;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    gap:40px;
    top: 67%;
    left: 5%;
}

.containerProgress .progress {
    position: relative;
    width: 35px;
    height: 35px;
    border-radius: 50%;
    color: #fff;
    background: #444 linear-gradient(to right, transparent 50%, var(--clr) 0);
}

.containerProgress .progress h3 {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 0.7em;
    z-index: 1;
    font-weight: 500;
}

.containerProgress .progress span {
    font-size: 0.6em;
    font-weight: 400;
}

.containerProgress .progress::before {
    content: '';
    display: flex;
    height: 100%;
    margin-left: 50%;
    transform-origin: left;
    border-radius: 0 100% 100% 0/50%;
}

.containerProgress .progress::after {
    content: '';
    position: absolute;
    inset: 3px;
    border-radius: 50%;
    background: #222;
}

.containerProgress .progress::before {
    background: var(--clr);
    transform: rotate(calc(((var(--i) - 50) * 0.01turn )));
}

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
    flex-wrap: wrap;
    position: relative;
    padding-top: 10px;
}

.flex-container > .filters{
    width: 200px;
    margin: 10px;
    height: 390px;
}

.flex-container > .movies{
    width: 1200px;
    min-width: 1200px;
    margin: 10px;
}

.filters {
    border: 1px solid #e3e3e3;
    min-width: 250px;
    width: 250px;
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
    margin:  0px 20px 20px 20px;
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
    padding: 18px 10px 12px;
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


@media (max-width: 1500px) {
    .flex-container > .movies {
        width: 100%;
        min-width: 100%;
    }

    .filters {
        min-width: 100%;
    }

    .flex-container > .filters {
        height: 100%;
    }
}

@media (max-width: 900px) {
    .float-first {
        float: none;
        padding-left: 30px;
        padding-right: 30px;
        padding-top: 20px;

    }
    .float-child {
        width: 100%;
        padding: 20px;
    }
    .title {
        padding-left: 30px;
    }
}
</style>
