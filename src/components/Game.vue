<template lang="pug">
div#game
    button#startbutton(@click="generateGrid") Start
    #score(v-show="score != 0 || running") Score: {{ score }} 
    GameGrid#grid(:rows="grid.rows")
    #touchArea(v-show="isTouchDevice() && running") Swipe here to move...
</template>

<script>
import GameGrid from "../components/GameGrid.vue";

export default {
    data: () => Object({
        running: false,
        score: 0,
        grid: {rows: []}
    }),
    methods: {
        // Start new game
        generateGrid: function() {
            this.running = true
            this.score = 0

            // Empty current grid, initialize each tile with value 0 and coordinates
            this.grid.rows = Array(4).fill().map((_, y) =>
                Array(4).fill().map((_, x) =>
                    Object({value: 0, x, y})))

            // Set some tiles to 2
            this.addNewTiles(2, 2)
        },
        addNewTiles: function(num, value) {
            const emptyTiles = [].concat.apply([], this.grid.rows).filter(tile => tile.value == 0)
            if (emptyTiles.length < num) {
                this.loseGame()
                return
            }
            console.log('empty tiles', emptyTiles)
            this.getRandomItems(emptyTiles, num, true).forEach(tile => {
                console.log('chose', tile)
                this.grid.rows[tile.y][tile.x].value = value
            })
        },
        // Game logic
        progressGame: function() {
            // Add a random tile after a move; either a two or a four
            if(Math.random() < 0.5)
                this.addNewTiles(1,2)
            else
                this.addNewTiles(1,4)
        },
        loseGame: function() {
            window.alert("You lose! Score: " + this.score)
            this.running = false
        },
        winGame: function() {
            window.alert("You win! Score: " + this.score)
            this.running = false
        },
        // Movement handling
        moveLeft: function() {
            for (let x = 1; x < 4; ++x) {
                for (let y = 0; y < 4; ++y) {
                    let row = this.grid.rows[y];
                    let cell = row[x];
                    if (cell.value == 0)
                        continue
                    for (let nx = x - 1; nx >= 0; --nx) {
                        let ncell = row[nx];
                        if (ncell.value == 0) {
                            this.swapCells(cell, ncell)
                            cell = this.grid.rows[y][nx]
                            continue
                        }
                        if (ncell.value == cell.value)
                            this.combineCells(cell, ncell)
                        break
                    }
                }
            }
            console.log("Left")
            this.progressGame()
        },
        moveRight: function() {
            for (let x = 2; x >= 0; --x) {
                for (let y = 0; y < 4; ++y) {
                    let cell = this.grid.rows[y][x]
                    if (cell.value == 0)
                        continue
                    for (let nx = x + 1; nx < 4; ++nx) {
                        let ncell = this.grid.rows[y][nx]
                        if (ncell.value == 0) {
                            this.swapCells(cell, ncell)
                            cell = this.grid.rows[y][nx]
                            continue
                        }
                        if (ncell.value == cell.value)
                            this.combineCells(cell, ncell)
                        break
                    }
                }
            }
            console.log("Right")
            this.progressGame()
        },
        moveDown: function() {
            for (let y = 2; y >= 0; --y) {
                for (let x = 0; x < 4; ++x) {
                    let cell = this.grid.rows[y][x]
                    if (cell.value == 0)
                        continue
                    for (let ny = y + 1; ny < 4; ++ny) {
                        let ncell = this.grid.rows[ny][x]
                        if (ncell.value == 0) {
                            this.swapCells(cell, ncell)
                            cell = this.grid.rows[ny][x]
                            continue
                        }
                        if (ncell.value == cell.value)
                            this.combineCells(cell, ncell)
                        break
                    }
                }
            }
            console.log("Down")
            this.progressGame()
        },
        moveUp: function() {
            for (let y = 1; y < 4; ++y) {
                for (let x = 0; x < 4; ++x) {
                    let cell = this.grid.rows[y][x]
                    if (cell.value == 0)
                        continue
                    for (let ny = y - 1; ny >= 0; --ny) {
                        let ncell = this.grid.rows[ny][x]
                        if (ncell.value == 0) {
                            this.swapCells(cell, ncell)
                            cell = this.grid.rows[ny][x]
                            continue
                        }
                        if (ncell.value == cell.value)
                            this.combineCells(cell, ncell)
                        break
                    }
                }
            }
            console.log("Up")
            this.progressGame()
        },
        // RNG helpers
        getRandomInt: function(min, max, inclusive) {
            min = Math.ceil(min);
            max = Math.floor(max);
            if (inclusive)
                max += 1;
            return Math.floor(Math.random() * (max - min)) + min;
        },
        getRandomItems: function(arr, num, exclusive) {
            if (num >= arr.length)
                return arr
            let items = []
            for (let n = 0; n < num; ++n)
            {
                let index = this.getRandomInt(0, arr.length);
                items.push(arr[index]);
                if (exclusive)
                    arr.splice(index, 1);
            }
            return items;
        },
        // Cell movement helpers
        swapCells: function(from, to) {
            let f = from.value
            let t = to.value
            this.grid.rows[to.y][to.x].value = f
            this.grid.rows[from.y][from.x].value = t
        },
        combineCells: function(from, to) {
            this.grid.rows[to.y][to.x].value += from.value
            this.grid.rows[from.y][from.x].value = 0
            const newValue = this.grid.rows[to.y][to.x].value
            // Increase score by resulting cell
            this.score += newValue
            // Win if a cell reaches 2048
            if (newValue >= 2048) {
                this.winGame()
            }
        },
        // Touchscreen check
        isTouchDevice: function() {  
            try {  
                document.createEvent("TouchEvent");  
                return true;  
            } catch (e) {  
                return false;  
            }  
        }
    },
    mounted () {
        // Keyboard input
        window.addEventListener("keydown", e => {
            if (!this.running) 
                return
            switch (e.keyCode) {
                case 37:
                    e.preventDefault()
                    this.moveLeft()
                    break;
                case 40:
                    e.preventDefault()
                    this.moveDown()
                    break;
                case 39:
                    e.preventDefault()
                    this.moveRight()
                    break;
                case 38:
                    e.preventDefault()
                    this.moveUp()
                    break;
            }
        });
        if(!this.isTouchDevice)
            return
        // Touchscreen input
        let initialX = null;
        let initialY = null;
        document.querySelector("#touchArea").addEventListener("touchstart", e => {
            initialX = e.touches[0].clientX
            initialY = e.touches[0].clientY
            e.preventDefault()
        })
        document.querySelector("#touchArea").addEventListener("touchmove", e => {
            if (initialX == null || initialY == null)
                return
            let currentX = e.touches[0].clientX
            let currentY = e.touches[0].clientY
            let diffX = initialX - currentX
            let diffY = initialY - currentY
            // sliding horizontally
            if (Math.abs(diffX) > Math.abs(diffY)) {
                if (diffX > 0) {
                    // swiped left
                    this.moveLeft()
                    console.log("swiped left");
                } else {
                    // swiped right
                    this.moveRight()
                    console.log("swiped right");
                }  
            // sliding vertically
            } else {
                if (diffY > 0) {
                    // swiped up
                    this.moveUp()
                    console.log("swiped up");
                } else {
                    // swiped down
                    this.moveDown()
                    console.log("swiped down");
                }  
            }
            initialX = null;
            initialY = null;
            e.preventDefault();
        })
    },
    components: {
        GameGrid
    }
}
</script>

<style lang="scss">
#game {
    text-align: center;
    font-family: monospace;
}

#score {
    font-family: inherit;
    margin: .5em;
    font-size: 1.2em;
}

#startbutton {
    min-width: fit-content;
    width: 8vw;
    font-size: 2em;
    border: 2px solid black;
    border-radius: 4px;
    &:hover {
        background-color: #EEE;
        cursor: pointer;
    }
}

#touchArea {
    $hpadding: 25vh;
    $wpadding: 75vw;

    margin: auto;
    margin-top: 1em;
    padding-top: $hpadding/2;
    background-color: #EEE;
    color: #AAA;
    width: $wpadding;
    height: $hpadding/2;
    border-radius: 4px;
}
</style>