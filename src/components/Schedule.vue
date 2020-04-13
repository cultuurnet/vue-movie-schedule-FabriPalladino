<template>
    <section>
        <div class="tr">
            <div class="td">
                
            <label
                :class="{ 'values-copied' : valuesOnClipboard.length }">
                <small></small>
            </label>
            </div>
            <div class="td td--header"
                v-for="(timeSlot, columnIndex) in maxTimestamps"
                :key="`schedule-header-${columnIndex}`">
                t{{++columnIndex}}

                <CopyBtn
                    @click.native="copyColumnValues(columnIndex)" />
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
                    @click="pasteValues(valuesOnClipboard, columnIndex, rowIndex)" />
            </div>
            <div class="td">

                <CopyBtn
                    @click.native="copyRowValues(rowIndex)" />
            </div>
        </div>
    </section>
</template>

<script>
import { format, eachDayOfInterval, add } from 'date-fns'
import CopyBtn from "@/components/CopyBtn"

export default {
    name: "movie-schedule",
    props: ["interval"],
    data: () => ({
        // We can add a property to change the number of max timestamps, it can be data retrieved from a backend for example
        maxTimestamps: 7,
        columnValuesCopied: false,
        rowValuesCopied: false,
        valuesOnClipboard: []
    }),
    components: {
        CopyBtn
    },
    computed: {
        dates () {
            const rawInterval = eachDayOfInterval({ 
                start: new Date(), 
                end: add(new Date(), {days: 7})
                });
            const dates = [];
            rawInterval.forEach(date => {
                dates.push(format(date, 'dd/MM/yyyy'))
            })
            return dates
        }
    },
    methods: {
        copyColumnValues (i) {
            /**
             * @param {Number} i - Index of the selected column
             */
            // Convert index to start again from 0
            const selectedColumn = --i;

            // Set copied mode on column
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
            /**
             * @param {Number} i - Index of the selected row
             */

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
            /**
             * A set of coordinates to match elements in the table.
             * @param {Array} values - The values currently saved, from a column or a row.
             * @param {Number} columnIndex - The index of the corresponding clicked column.
             * @param {Number} rowIndex - The index of the corresponding clicked row.
             */
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
    position: relative;
    background-color: #ccc;
    display: table;
    width: 100%;
    max-width: 80vw;
    margin: 0 auto;
    padding: 2.5vmin 0;
    z-index: 10;

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

    label {
        display: inline-block;

        &.values-copied {
            small {
                background: green;
                transition: .3s;

                &:after {
                    content: "Values copied";
                    padding: 0 12px;
                }
            }
        }

        small {
            display: inline-block;
            width: 100px;
            height: 18px;
            background: #455a64;
            border-radius: 30px;
            position: relative;
            padding: 5px 0;

            &:after {
                content: "Clipboard empty";
                position: absolute;
                color: #fff;
                font-size: 11px;
                font-weight: 600;
                width: 100%;
                left: 0px;
                text-align: right;
                padding: 0 5px;
                box-sizing: border-box;
                line-height: 18px;
            }
        }
    }
}
</style>