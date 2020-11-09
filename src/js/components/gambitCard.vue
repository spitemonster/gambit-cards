<template>
    <div :class="[gambit.school, gambit.broken]" class="gambit__card" :data-key="gambit['Name']" @click="select">
        <h3 class="gambit__title">{{ gambit.name }}</h3>
        <div class="split">
            <div class="stat"><span>Pull Time</span><span>{{ gambit.pull }}</span></div>
            <div class="stat"><span>Range</span><span>{{ gambit.range }}</span></div>
        </div>
        <div class="split">
            <div class="stat"><span>Components</span><span><template v-if="gambit.components === 'None'">{{ gambit.components }}</template><template v-else>Yes</template></span></div>
            <div class="stat"><span>Duration</span><span>{{ gambit.duration }}</span></div>
        </div>
        <div v-if="gambit.components !== 'None'" class="stat components">
            <span>{{ gambit.components }}</span>
        </div>
        <div class="gambit__description">
            <p v-html="gambit.description"></p>
            <p class="gambit__higher"> <b v-if="gambit.higher">At Higher Levels: </b>{{ gambit.higher }}</p><span class="gambit__note">{{ gambit.note }}</span>
        </div>
        <div class="gambit__info" v-if="gambit.level !== 'Trick'">
            <span>{{ gambit.level }}-Level {{ gambit.school }} Gambit</span>
        </div>
        <div class="gambit__info" v-else>
            <span>{{ gambit.school }} Trick</span>
        </div>
    </div>
</template>
<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';

interface Gambit {
    name: string;
    level: string;
    school: string;
    range: string;
    pull: string;
    note ? : string;
    duration: string;
    components: string;
    description: string;
    tags: string[];
}

@Component
export default class GambitCard extends Vue {

    constructor() {
        super();
    }

    @Prop() gambit: Gambit;

    select() {
        console.log('select');
    }
}

</script>
<style lang="css">
</style>
