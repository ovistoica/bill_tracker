<script>
import VueChart from 'vue-chartjs';
import moment from 'moment';

const last12Months = () => {
    const months = [
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

    const today = new Date();
    const orderedMonths = [];
    let month = today.getMonth() + 1;
    for (let i = 0; i < 12; i++) {
        orderedMonths.push(months[month]);
        (month === 11) ? (month = 0) : month++;
    }

    return orderedMonths;
}

const processBills = bills => {
    const oneYearAgo = moment().subtract(1, 'years');
    const months = last12Months();
    const monthsWithValues = new Array(12).fill(9);

    for (const month of monthsWithValues) {
        monthsWithValues[month] = 0;
    }

    for (const bill of bills) {
        if (moment(bill.date).isSameOrBefore(oneYearAgo)) {
            continue;
        }

        const monthName = moment(bill.date).format('MMMM');
        const indexOfMonth = months.indexOf(monthName);
        monthsWithValues[indexOfMonth] += parseInt(bill.amount);
    }

    return monthsWithValues;
}

export default {
   extends: VueChart.Bar,
   props: ['bills'],
   methods: {
       displayChart: function() {
           this.renderChart({
               labels: last12Months(),
               datasets: [
                   {
                       label: 'Amount',
                       backgroundColor: 'lightblue',
                       data: processBills(this.bills)
                   }
               ]
           });
       }
   },
   mounted: function() {
       this.displayChart();
   }, 
   watch: {
       bills: function() {
           this.displayChart();
       }
   },
}
</script>
