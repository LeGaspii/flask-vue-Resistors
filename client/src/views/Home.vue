<template>
<div>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <a class="navbar-brand" href="#">Resistors</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse"
            data-target="#navbarNav" aria-controls="navbarNav"
            aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
        <li class="nav-item active">
            <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
        </li>
        <!-- <li class="nav-item">
            <a class="nav-link" href="#">Features</a>
        </li>
        <li class="nav-item">
            <a class="nav-link" href="#">Pricing</a>
        </li>
        <li class="nav-item">
            <a class="nav-link disabled" href="#">Disabled</a>
        </li> -->
        </ul>
    </div>
    </nav>

    <div class="w-full lg:w-6/12 px-4 ml-auto mr-auto text-center">

        <h1 class="text-white font-semibold text-5xl mb-2">
            Tu cherches une valeur standard ?
        </h1>
        <div class="bg-white w-100 bg-opacity-75 p-2 rounded-lg">
            <h2>Resistances</h2>
            <p>Type a number between 1 and 10.</p>
            <!-- <input type="number" id="myNumber" value="0"> -->
            <!-- <p>Type a number between {{ ResistValue}} and 10.</p> -->
            <input id="myNumber"  v-model="ResistValue"
                    type='number'>
            <button @click="resistanceValues()">Try it</button>

            <div class="w-100">
            <table id="table" class="table-auto p-2 m-2">
            <thead id="series"></thead>
            <tbody id="results"></tbody>
            </table>
            </div>
        </div>
    </div>
</div>
</template>

<script>
// import CalcResist from '../components/CalcResist.vue';

export default ({
  component: {
    // CalcResist,
  },
  data() {
    return {
      closestValuesForE: {
        keys: [12, 24, 48, 96, 192],
        EList: ['E12', 'E24', 'E48', 'E96', 'E192'],
        values: [0, 0, 0, 0, 0],
      },
      ResistValue: 1.5,
    };
  },
  methods: {
    resistanceValues() {
      document.querySelectorAll('#table tr').forEach((e) => { e.remove(); });

      const val = parseFloat(document.getElementById('myNumber').value, 10);
      // const closestValuesForE = {
      //   keys: [12, 24, 48, 96, 192],
      //   EList: ['E12', 'E24', 'E48', 'E96', 'E192'],
      //   values: [0, 0, 0, 0, 0],
      // };

      let arrayE = [];
      let i;
      console.log(this.closestValuesForE.keys);
      // eslint-disable-next-line no-restricted-syntax
      for (i in this.closestValuesForE.keys) {
        if (i) {
          const E = this.closestValuesForE.keys[i];
          console.log(E);
          for (i = 0; i <= E; i += 1) {
            if (E <= 24) {
              arrayE[arrayE.length] = Math.round((10 ** (i / E)) * 10) / 10;
              console.log('inf 24', arrayE[arrayE.length]);
            } else {
              arrayE[arrayE.length] = Math.round((10 ** (i / E)) * 100) / 100;
              console.log(arrayE[arrayE.length]);
            }
          }
          console.log('val:', val);
          this.closestValuesForE.values[i] = this.closest(arrayE, val, false);
          arrayE = [];
        }
      }

      this.addARow('series', 'Series', this.closestValuesForE.EList);
      //   _ = this.closest(closestValuesForE.values, val, true);
      this.addARow('results', 'Closest value for each series', this.closestValuesForE.values);
      this.addARow('results', '%error', this.error(this.closestValuesForE.values, val));
      document.querySelectorAll('#table td').forEach((e) => { e.classList.add('border-4', 'px-4', 'py-4'); });
      document.querySelectorAll('#series td').forEach((e) => { e.classList.add('bg-yellow-200'); });
    },

    closest(array, num, fillClosestRow) {
      let i = 0;
      let minDiff = 1000;
      let ans;
      let savedI = 0;
      const closestRow = [0, 0, 0, 0, 0];
      console.log('inside closest');
      // eslint-disable-next-line no-restricted-syntax
      for (i in array) {
        if (i) {
          const m = Math.abs(num - array[i]);
          if (m < minDiff) {
            minDiff = m;
            ans = array[i];
            savedI = i;
          }
        }
      }
      closestRow[savedI] = ans;
      if (fillClosestRow) {
        this.addARow('results', 'Closest Resistor (R1)', closestRow);
      }
      return ans;
    },

    error(items, target) {
      const ans = [];
      let i = 0;
      items.forEach((item) => {
        ans[i] = Math.abs(Math.round((100 - ((item * 100) / target)) * 100) / 100);
        i += 1;
      });
      return ans;
    },

    addARow(tableId, explanations, items) {
      const table = document.getElementById(tableId);
      const rowNode = document.createElement('tr');

      let cellNode = document.createElement('td');
      let textNode = document.createTextNode(explanations);
      cellNode.appendChild(textNode);
      rowNode.appendChild(cellNode);

      items.forEach((item) => {
        cellNode = document.createElement('td');
        if (item !== 0) {
          textNode = document.createTextNode(item);
          cellNode.appendChild(textNode);
        }
        rowNode.appendChild(cellNode);
      });
      table.appendChild(rowNode);
    },
  },
});
</script>
