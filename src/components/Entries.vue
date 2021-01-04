<template>
    <div>
        <button @click="switchNewestToOldest" v-if="!newestToOldest">Newest to oldest</button>
        <button @click="switchNewestToOldest" v-else>Oldest to newest</button>

        <div v-for="entry in sortedEntries" :key="entry">
            <h1>{{entry.title}}</h1>
            <small>{{entry.date | formatDate}}</small><br>
            <img :src=entry.image>
            <p>{{entry.text}}</p>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Entries',
        props: {
            entries: Array
        },
        computed: {
            sortedEntries: function () {
                let array = this.entries;
                array.sort( ( a, b) => {
                    return new Date(a.date) - new Date(b.date);
                });
                if (this.newestToOldest) {
                    return array;
                } else {
                    return array.reverse()
                }
            }

        },
        filters: {
            formatDate: function (value) {
                if (!value) return ''
                let date = new Date(value);
                let month = date.getMonth();
                let day = date.getDate();
                let year = date.getFullYear();

                let months = [
                    'January',
                    'February',
                    'March',
                    'April',
                    'May',
                    'June',
                    'July',
                    'August',
                    'September',
                    'October',
                    'November',
                    'December'
                ]

                let m = months[month];
                let d =  '';
                if (day == 11 || day == 12 || day == 13) {
                    d = day + 'th'
                }  else {
                    switch (day % 10) {
                        case 1:
                            d = day + 'st'
                            break;
                        case 2:
                            d = day + 'nd'
                            break;
                        case 3:
                            d = day + 'rd'
                            break;
                        default:
                            d = day + 'th'
                    }
                }
                let hours = date.getHours()
                if (hours.toString().length == 1) {
                    hours = "0" + hours
                }
                let time = hours + ":" + date.getMinutes()
                return m + " " + d + ", " + year + " " + time

            }
        },
        methods: {
            switchNewestToOldest: function () {
                this.newestToOldest = !this.newestToOldest
            }
        },
        data: function () {
            return {
                newestToOldest: true
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h3 {
        margin: 40px 0 0;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    li {
        display: inline-block;
        margin: 0 10px;
    }

    a {
        color: #42b983;
    }
</style>
