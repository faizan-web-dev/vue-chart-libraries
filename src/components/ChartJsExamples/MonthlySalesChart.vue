<template>
	<div>
		<div class="field is-grouped">
			<div class="control">
				<label class="checkbox">
          <input type="checkbox" :value="2017" v-model="selectedYears">
          2017 Sales
        </label>
				<div class="control">
					<label class="checkbox">
          <input type="checkbox" :value="2018" v-model="selectedYears">
          2018 Sales
        </label>
				</div>
			</div>
			<div class="control">
				<label class="checkbox">
          <input type="checkbox" :value="2019" v-model="selectedYears">
          2019 Sales
        </label>
			</div>
		</div>

		<line-chart :width="300" :height="200" :labels="['Jan', 'Feb', 'Mar', 'Apr', 'May']"
			:datasets="displayedDatasets" :options="$options.options"></line-chart>
		<h2>Bar Chart</h2>
		<line-chart type="bar" :width="300" :height="200" :labels="['Jan', 'Feb', 'Mar', 'Apr', 'May']"
			:datasets="displayedDatasets" :options="$options.options" />
    <h2>Pie Chart</h2>
		<line-chart type="pie" :width="300" :height="200" :labels="['Jan', 'Feb', 'Mar', 'Apr', 'May']"
			:datasets="displayedDatasets" :options="$options.options" />
	</div>
</template>

<script>
	import numeral from 'numeral';
import LineChart from './LineChart';

const datasets = {
    2017: {
    label: '2017 Sales',
    borderColor: 'rgba(255, 56, 96, 0.5)',
    backgroundColor: 'rgba(255, 56, 96, 0.1)',
    data: [600, 550, 750, 250, 700]
  },
  2018: {
    label: '2018 Sales',
    borderColor: 'rgba(50, 115, 220, 0.5)',
    backgroundColor: 'rgba(50, 115, 220, 0.1)',
    data: [300, 700, 450, 750, 450]
  },
  2019: {
    label: '2019 Sales',
    borderColor: '#20c997',
    backgroundColor: '#15h993',
    data: [100, 350, 300, 250, 400]
  }
};

const options = {
  scales: {
    yAxes: [{
      ticks: {
        beginAtZero: true,
        callback: value => numeral(value).format('$0,0')
      }
    }]
  },
  tooltips: {
    mode: 'index',
    callbacks: {
      label(tooltipItem, {datasets}) {
        const label = datasets[tooltipItem.datasetIndex].label;
        const value = numeral(tooltipItem.yLabel).format('$0,0');

        return `${label}: ${value}`;
      }
    }
  },
  responsive: true,
  maintainAspectRatio: true
};

export default {
  name: 'MonthlySalesChart',
  datasets,
  options,
  components: {
    LineChart,
  },
  data() {
    return {
      selectedYears: [2017, 2018, 2019]
    };
  },
  computed: {
    displayedDatasets() {
      return this.selectedYears.map(year => datasets[year]);
    }
  }
}
</script>