<template>
    <div>
        <nav class="navbar navbar-expand-lg bg-body-tertiary">
            <div class="container-fluid">
                <a class="navbar-brand text-redcollor" href="#">Yu gi oh!!</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
                </button>
                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                    <li class="nav-item">
                        <router-link to="/">
                            <a class="nav-link active text-redcollor" aria-current="page" href="#">Home</a>
                        </router-link>
                    </li>
                    <li class="nav-item">
                        <router-link to="/cart">
                            <a class="nav-link text-redcollor" href="#">Carrinho</a>
                        </router-link>
                    </li>
                </ul>
                </div>
            </div>
        </nav>
    </div>
</template>
<script lang="ts">
export default {
    name: "NavBar"
}
</script>

<style scoped>
.text-redcollor {
    color: #d31417 !important;
}

</style>
