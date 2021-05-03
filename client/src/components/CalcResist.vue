<template>
    <div class="w-full lg:w-6/12 px-4 ml-auto mr-auto text-center">

        <h1 class="text-white font-semibold text-5xl mb-2">
            Tu cherches une valeur standard ?
        </h1>
        <div class="bg-white w-100 bg-opacity-75 p-2 rounded-lg">
            <h2>Resistances</h2>
            <p>Type a number between 1 and 10.</p>
            <input type="number" id="myNumber" value="0">
            <button onclick="resistanceValues()">Try it</button>

            <div class="w-100">
            <table id="table" class="table-auto p-2 m-2">
            <thead id="series"></thead>
            <tbody id="results"></tbody>
            </table>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'CalcResist',
};
</script>
