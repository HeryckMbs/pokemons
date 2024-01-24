<template>
    <div class="doubleDamageContainer" v-if="Object.keys(arrayDamage).length > 0">
        <h4>{{ title }}</h4>
        <div class="baseContainer doubleDamage">
            <div :class="[index, 'badge']" :key="index" v-for="(item, index) in arrayDamage">
                <p> {{ index }}</p>
            </div>
        </div>
    </div>
</template>

<script>
export default{
    props:{
        arrayDamage: Object,
        title: String
    }
}
</script>

<style>
.doubleDamageContainer {
    width: 50%;
    height: 100%;
    margin-right: 5%;

}

.doubleDamage {
    display: grid;
    grid-template-areas: 'item item';
    grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));

    gap: 5%;
    padding: 5%;
    width: 100%;

}
</style>