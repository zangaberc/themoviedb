<template>
   <div>
        <div class="footer">
            <div class="container">
                <div class="row">
                    <div class="col">
                        <h4>THE BASICS</h4>
                        <ul>
                            <li><a href="#">About TMDB</a></li>
                            <li><a href="#">Contact Us</a></li>
                            <li><a href="#">Support Forums</a></li>
                            <li><a href="#">API</a></li>
                            <li><a href="#">System Status</a></li>
                        </ul>
                    </div>
                    <div class="col">
                        <h4>GET INVOLVED</h4>
                        <ul>
                            <li><a href="#">Contribution Bible</a></li>
                            <li><a href="#">Add New Movie</a></li>
                            <li><a href="#">Add New TV Show</a></li>
                        </ul>
                    </div>
                    <div class="col">
                        <h4>COMMUNITY</h4>
                        <ul>
                            <li><a href="#">Guidelines</a></li>
                            <li><a href="#">Discussions</a></li>
                            <li><a href="#">Leaderboard</a></li>
                        </ul>
                    </div>
                    <div class="col">
                        <h4>LEGAL</h4>
                        <ul>
                            <li><a href="#">Terms of Use</a></li>
                            <li><a href="#">API Terms of Use</a></li>
                            <li><a href="#">Privacy Policy</a></li>
                            <li><a href="#">Leaderboard</a></li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
   </div>
</template>
<style scoped>
.container {
    max-width: 1170px;
    margin: auto;
}

.footer {
    background: #032541;
    padding: 70px 0;
}

.row {
    display: flex;
    flex-wrap: wrap;
}

.col {
    width: 25%;
    padding: 0 15px;
}

.col h4 {
    font-size: 18px;
    color: #fff;
    margin-bottom: 35px;
    font-weight: 500;
    position: relative;
}

.col h4::before {
    content: '';
    position: absolute;
    left: 0;
    bottom: -10px;
    background: #01b4e4;
    height: 2px;
    box-sizing: border-box;
    width: 50px;
}

.col ul {
    list-style: none;
    padding-left: 2px;
}

.col ul li a {
    font-size: 16px;
    text-decoration: none;
    color: #fff;
    display: contents;
    transition: all .3s ease;
}

.col ul li a:hover {
    color: #fff;
    padding-left: 6px;
}

@media (max-width: 767px) {
    .col {
        width: 50%;
        margin-bottom: 30px
    }
}

@media (max-width: 567px) {
    .col {
        width: 100%;
    }
}
</style>