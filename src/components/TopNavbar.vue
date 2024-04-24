<template>
    <header>
        <img class="logo" src="..\logo\logo.svg" alt="logo">
        <nav>
            <ul class="nav_links">
                <li><a href="#">Movies</a></li>
                <li><a href="#">TV Shows</a></li>
                <li><a href="#">People</a></li>
                <li><a href="#">More</a></li>
            </ul>
        </nav>
        <a class="cta" href="#"><button>Contact</button></a>
    </header>
</template>
<script>
</script>
<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 30px 10%;
  background-color: #032541;
}

li, a, button {
    font-weight: 600;
    font-size: 20px;
    color: #fff;
    text-decoration: none;
}

.logo {
    cursor: pointer;
    width: 150px;
}

.nav_links {
    list-style: none;
}

.nav_links li {
    display: inline-block;
    padding: 0px 20px;
}

.nav_links li a {
    transition: all 0.3s ease 0s;
}

.nav_links li a:hover {
    color: #0088a9;
}

button {
    padding: 9px 25px;
    background-color: rgba(0, 136, 169, 1);
    border: none;
    border-radius: 50px;
    cursor: pointer;
    transition: all 0.3s ease 0s;
}

button:hover {
    background-color: rgba(0, 136, 169, 0.8);
}

.icons {
    position: absolute;
    font-size: 2.8rem;
    color: #fff;
    cursor: pointer;
    display: none;
}

@media (max-width: 992px) {
    header {
        padding: 1.3rem 5%;
    }
}

@media (max-width: 768) {
    .icons {
        display: block;
    }
}

</style>