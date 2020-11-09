<template>
    <div>
        <div class="filter-button" @click="showFilter">
            <i class="las la-filter"></i>
        </div>
        <div class="gambit__filter">
            <input type="text" placeholder="Search" v-model="search" class="search">
            <select id="sort" v-model="gambitSort">
                <option value="Name">Title</option>
                <option value="level">Level</option>
                <option value="school">School</option>
            </select>
            <div class="show-schools">
                <h3>Schools</h3>
                <label v-for="school, k in showSchools"><input type="checkbox" v-model="showSchools[k]"><span></span>{{ k }}</label>
            </div>
            <div class="show-levels">
                <h3>Levels</h3>
                <label v-for="level, k in showLevels"><input type="checkbox" v-model="showLevels[k]"><span></span>{{ k }}</label>
            </div>
            <div class="show-tags">
                <h3>Tags</h3>
                <label v-for="tag, k in showTags"><input type="checkbox" v-model="showTags[k]"><span></span>{{ k }}</label>
            </div>
            <div class="button-row">
                <button @click="filterGambits">Apply</button>
                <button @click="resetFilter">Reset</button>
            </div>
        </div>
        <div class="gambit__wrap">
            <div v-for="n in this.pages" class="page">
                <GambitCard v-for="gambit, i in filteredGambits.slice((n - 1) * 8, (n-1) * 8 + 8)" :key="i" :gambit="gambit"></GambitCard>
            </div>
        </div>
    </div>
</template>
<script lang="ts">
import { Component, Vue, Watch } from 'vue-property-decorator';
import GambitCard from './components/gambitCard.vue';

import gambits from './gambits.json'

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

@Component({
    components: {
        GambitCard
    }
})
export default class App extends Vue {
    // declare initial data types here
    gambits: Gambit[];
    filteredGambits: object[];
    search: string;
    gambitSort: string;
    showLevels: object;
    showSchools: object;
    showTags: object;

    constructor() {
        super();

        // here is where we initialize data; not with their starting data but with empty versions of their types

        this.gambits = gambits.gambits;
        this.search = '';
        this.gambitSort = 'school';
        this.showLevels = {
            Trick: true,
            "1st": true,
            "2nd": true,
            "3rd": true,
            "4th": true,
            "5th": true,
            "6th": true,
            "7th": true,
            "8th": true,
            "9th": true
        }

        this.showSchools = {
            Civility: true,
            Crafting: true,
            Daredevil: true,
            Foresight: true,
            Instinct: true,
            "Old Ways": true,
            Patching: true,
            Scrounge: true,
            Slaying: true,
            Wilderness: true
        }

        this.showTags = {
            buff: true,
            charm: true,
            compulsion: true,
            control: true,
            crafting: true,
            damage: true,
            debufff: true,
            defense: true,
            healing: true,
            movement: true,
            repair: true,
            utility: true,
        }

        this.filteredGambits = [];
    }

    created() {
        // we can initialize starting data values here
    }

    mounted() {
        this.filteredGambits = [...this.gambits];
        this.sortByLevel(this.filteredGambits);
        this.sortBySchool(this.filteredGambits);

    }

    get pages() {
        return Math.ceil(this.filteredGambits.length / 8);
    }

    //methods
    filterGambits() {
        this.filteredGambits = this.gambits.filter((gambit) => {
            let level = gambit.level;
            let school = gambit.school;
            let name = gambit.name.toLowerCase();

            if (this.showLevels[level] && this.showSchools[school] && this.tagsSelected(gambit) && name.includes(this.search.toLowerCase())) {
                return true;
            }

            return false;
        })

        this.sortGambits();

        this.showFilter();
    }

    resetFilter() {
        this.filteredGambits = this.gambits;
        this.search = '';
        this.gambitSort = 'school';
        this.showLevels = {
            Trick: true,
            "1st": true,
            "2nd": true,
            "3rd": true,
            "4th": true,
            "5th": true,
            "6th": true,
            "7th": true,
            "8th": true,
            "9th": true
        }

        this.showSchools = {
            Civility: true,
            Crafting: true,
            Daredevil: true,
            Foresight: true,
            Instinct: true,
            "Old Ways": true,
            Patching: true,
            Scrounge: true,
            Slaying: true,
            Wilderness: true
        }

        this.showTags = {
            buff: true,
            charm: true,
            compulsion: true,
            control: true,
            crafting: true,
            damage: true,
            debufff: true,
            defense: true,
            healing: true,
            movement: true,
            repair: true,
            utility: true,
        }

        this.showFilter()
    }

    searchGambits() {
        this.filteredGambits = this.filteredGambits.filter((gambit) => {
            if (gambit['name'].toLowerCase().includes(this.search.toLowerCase())) {
                return true
            }
        })
    }

    tagsSelected(gambit) {
        let tags = false;

        for (let i = 0, l = gambit["tags"].length; i < l; ++i) {
            let t = gambit["tags"][i]

            if (this.showTags[t]) {
                tags = true;
                break
            }

        }
        return tags;
    }

    sortGambits() {
        let heuristic = this.gambitSort;

        if (heuristic === 'Name') {
            this.sortByTitle(this.filteredGambits)
        } else if (heuristic === 'level') {
            this.sortByLevel(this.filteredGambits)
        } else if (heuristic === 'school') {
            this.sortBySchool(this.filteredGambits)
        }
    }

    showFilter() {
        let filter = this.$el.querySelector('.gambit__filter')

        if (filter.classList.contains('show')) {
            return filter.classList.remove('show');
        }

        filter.classList.add('show');
    }

    sortByTitle(arr) {

        console.log(arr);

        arr.sort((a, b) => {
            if (a.name == b.name) {
                return 0;
            } else if (a.name > b.name) {
                return 1;
            } else if (a.name < b.name) {
                return -1;
            }
        })
    }

    sortBySchool(arr) {
        arr.sort((a, b) => {
            if (a['school'] == b['school']) {
                return 0;
            } else if (a['school'] > b['school']) {
                return 1;
            } else if (a['school'] < b['school']) {
                return -1;
            }
        })
    }

    sortByLevel(arr) {
        arr.sort((a, b) => {
            let aLev = a['level'] == 'Trick' ? "0th" : a['level']
            let bLev = b['level'] == 'Trick' ? "0th" : b['level']

            if (aLev == bLev) {
                return 0;
            } else if (aLev < bLev) {
                return -1;
            } else if (aLev > bLev) {
                return 1;
            }
        })
    }
}

</script>
