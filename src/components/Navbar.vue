<template>
    <nav class="w-full h-20 shadow fixed top-0 left-0 right-0 bg-white flex justify-center">
        <div class="container mx-auto flex justify-between items-center">
            <router-link to="/">
                <p><span class="font-bold mr-2 text-orange">XP</span>CREDENTIALING</p>
            </router-link>

            <div class="flex items-center">
                <span class="font-bold text-gray-900">Welcome Admin John Doe</span>
                <img src="../assets/img/img-user.jpg" class="h-7 w-7 rounded-full ml-5">
            </div>
        </div>
    </nav>
    <router-view/>
</template>

<script>
export default {

}
</script>