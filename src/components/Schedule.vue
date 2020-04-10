<template>
    <section>
        <div class="tr">
            <div class="td"></div>
            <div class="td td--header"
                v-for="(timeSlot, columnIndex) in maxTimestamps"
                :key="`schedule-header-${columnIndex}`">
                t{{++columnIndex}}
                <button
                    @click="copyColumnValues(columnIndex)"> 
                    <svg x="0px" y="0px" viewBox="0 0 96 96" width="25" height="25">
                        <path d="M72.6,32.3c0.8,0,1.5,0.3,2.1,0.9c0.6,0.6,0.9,1.3,0.9,2.1v37.3c0,0.8-0.3,1.5-0.9,2.1   s-1.3,0.9-2.1,0.9H43.1c-0.8,0-1.5-0.3-2.1-0.9s-0.9-1.3-0.9-2.1v-8.8H23.4c-0.8,0-1.5-0.3-2.1-0.9c-0.6-0.6-0.9-1.3-0.9-2.1V40.1   c0-0.8,0.2-1.7,0.6-2.7c0.4-1,0.9-1.8,1.5-2.3l12.5-12.5c0.6-0.6,1.4-1.1,2.3-1.5c1-0.4,1.9-0.6,2.7-0.6h12.8   c0.8,0,1.5,0.3,2.1,0.9s0.9,1.3,0.9,2.1v10.1c1.4-0.8,2.7-1.2,3.9-1.2H72.6z M55.9,38.8L46.7,48h9.2V38.8z M36.2,27L27,36.2h9.2V27   z M42.2,46.9l9.7-9.7V24.4H40.1v12.8c0,0.8-0.3,1.5-0.9,2.1s-1.3,0.9-2.1,0.9H24.4v19.6h15.7v-7.9c0-0.8,0.2-1.7,0.6-2.7   C41.2,48.2,41.7,47.5,42.2,46.9z M71.6,71.6V36.2H59.8V49c0,0.8-0.3,1.5-0.9,2.1c-0.6,0.6-1.3,0.9-2.1,0.9H44.1v19.6H71.6z"/>
                    </svg>
                </button>
            </div>
        </div>
        <div class="tr"
            v-for="(date, rowIndex) in dates"
            :key="`date-${rowIndex}`"
            :ref="`row-${rowIndex}`" >
            <div class="td">
                {{date}}
            </div>
            <div class="td"
                v-for="(timeSlot, columnIndex) in maxTimestamps"
                :key="`timeslot-${columnIndex}`">
                <input 
                    :ref="`column-${columnIndex}`" 
                    type="time" 
                    min="09:00" 
                    max="23:00"
                    @click="pasteValues(valuesOnClipboard, columnIndex, rowIndex)" />
            </div>
            <div class="td">
                <button
                    @click="copyRowValues(rowIndex)"> 
                    <svg x="0px" y="0px" viewBox="0 0 96 96" width="25" height="25">
                        <path d="M72.6,32.3c0.8,0,1.5,0.3,2.1,0.9c0.6,0.6,0.9,1.3,0.9,2.1v37.3c0,0.8-0.3,1.5-0.9,2.1   s-1.3,0.9-2.1,0.9H43.1c-0.8,0-1.5-0.3-2.1-0.9s-0.9-1.3-0.9-2.1v-8.8H23.4c-0.8,0-1.5-0.3-2.1-0.9c-0.6-0.6-0.9-1.3-0.9-2.1V40.1   c0-0.8,0.2-1.7,0.6-2.7c0.4-1,0.9-1.8,1.5-2.3l12.5-12.5c0.6-0.6,1.4-1.1,2.3-1.5c1-0.4,1.9-0.6,2.7-0.6h12.8   c0.8,0,1.5,0.3,2.1,0.9s0.9,1.3,0.9,2.1v10.1c1.4-0.8,2.7-1.2,3.9-1.2H72.6z M55.9,38.8L46.7,48h9.2V38.8z M36.2,27L27,36.2h9.2V27   z M42.2,46.9l9.7-9.7V24.4H40.1v12.8c0,0.8-0.3,1.5-0.9,2.1s-1.3,0.9-2.1,0.9H24.4v19.6h15.7v-7.9c0-0.8,0.2-1.7,0.6-2.7   C41.2,48.2,41.7,47.5,42.2,46.9z M71.6,71.6V36.2H59.8V49c0,0.8-0.3,1.5-0.9,2.1c-0.6,0.6-1.3,0.9-2.1,0.9H44.1v19.6H71.6z"/>
                    </svg>
                </button>
            </div>
        </div>
    </section>
</template>

<script>

export default {
    name: "movie-schedule",
    data: () => ({
        // We can add a property to change the number of max timestamps, it can be data retrieved from a backend for example
        maxTimestamps: 7,
        /**
         * For the sake of simplicity, I worked with an array of dates. 
         * Those data should reflect the current week or come from a previous selection (ie. a dropdown menu or similar)
         */
        dates: [
            "03-03-20",
            "04-03-20",
            "05-03-20",
            "06-03-20",
            "07-03-20",
            "08-03-20",
            "09-03-20",
        ],
        columnValuesCopied: false,
        rowValuesCopied: false,
        valuesOnClipboard: []
    }),
    methods: {
        copyColumnValues (i) {
            // Convert index to start again from 0
            const selectedColumn = --i;

            // Set copied mode on 
            this.columnValuesCopied = true;

            // Retrive inputs value for the selected column 
            const columnInputsArray = this.$refs[`column-${selectedColumn}`];
            let columnValuesArray = [];
            columnInputsArray.forEach(input => {
                columnValuesArray.push(input.value)
            })
            this.valuesOnClipboard = columnValuesArray
        },
        copyRowValues (i) {
            // Set copied mode on 
            this.rowValuesCopied = true;

            // Retrive inputs value for the selected row 
            const selectedRow = this.$refs[`row-${i}`][0];
            const rowInputsArray = selectedRow.querySelectorAll("input");

            let rowValuesArray = [];

            rowInputsArray.forEach(input => {
                rowValuesArray.push(input.value)
            })
            this.valuesOnClipboard = rowValuesArray     
        },
        pasteValues (values, columnIndex, rowIndex) { 
            if(!values.length) {
                return
            } else if(this.columnValuesCopied) {
               const columnInputsArray = this.$refs[`column-${columnIndex}`]; 

            //    Loop trought evry input in the selected column and assign values 
               for(let i=0; i < columnInputsArray.length; ++i) {
                   columnInputsArray[i].value = values[i]
               }
               this.columnValuesCopied = false
            } else {
                const selectedRow = this.$refs[`row-${rowIndex}`][0]; 
                const rowInputsArray = selectedRow.querySelectorAll("input");
                for(let i=0; i < rowInputsArray.length; ++i) {
                    rowInputsArray[i].value = values[i]
                }
                this.rowValuesCopied = false;
                
            }
               this.valuesOnClipboard = []
        }
    }
}
</script>

<style lang="scss" scoped>
section {
    background-color: #ccc;
    display: table;
    width: 100%;
    max-width: 80vw;
    margin: 0 auto;
    padding: 2.5vmin 0;

    .tr {
        display: table-row;

        .td {
            display: table-cell;

            &:first-child,
            &.td--header {
                font-weight: bold;
            }
        }
    }

    button {
        cursor: pointer;
        background: none;
        box-shadow: none;
        border: 1px solid transparent;

        &:hover {
            border: 1px solid #555;
        }
    }
}
</style>