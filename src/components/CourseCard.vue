<template>
<div class="card">

<h3>{{ course.title }}</h3>

<p><strong>Chef:</strong> {{ course.chef }}</p>

<p><strong>Level:</strong> {{ course.level }}</p>

<p><strong>Price:</strong> R{{ course.price }}</p>

<p v-if="course.available" class="available">
Available
</p>

<p v-else class="soldout">
Sold Out
</p>

<button
@click="saveCourse"
:disabled="!course.available"
>
Save To Wishlist
</button>

</div>
</template>

<script setup>

const props = defineProps({
course:Object
})

const emit = defineEmits(["save"])

const saveCourse = () => {
emit("save")
}

</script>

<style scoped>

.card{
padding:20px;
border:1px solid #ddd;
border-radius:10px;
}

.available{
color:green;
font-weight:bold;
}

.soldout{
color:red;
font-weight:bold;
}

</style>