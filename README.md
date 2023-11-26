#PhysioTools

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sit to Stand Power Calculator</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f4f4f4;
            color: #333;
            margin: 20px;
        }

        h2 {
            color: #007BFF;
        }

        form {
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
        }

        input {
            width: 100%;
            padding: 8px;
            margin-bottom: 16px;
            box-sizing: border-box;
            border: 1px solid #ccc;
            border-radius: 4px;
        }

        button {
            background-color: #007BFF;
            color: #fff;
            padding: 10px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }

        button:hover {
            background-color: #0056b3;
        }

        #result {
            margin-top: 20px;
        }

        #referenceTableContainer {
            margin-top: 20px;
            overflow: hidden;
        }

        #referenceTable {
            width: 100%;
            max-width: 800px;
            border-collapse: collapse;
            margin: 0 auto;
            display: none; /* Hide initially */
        }

        #referenceTable th, #referenceTable td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: center;
        }

        #referenceTable th {
            background-color: #f2f2f2;
        }

        #lineChart {
            max-width: 800px;
            margin: 20px auto;
        }
    </style>
</head>
<body>

<h2>Sit to Stand Power Calculator</h2>

<form id="sitToStandForm">
    <label for="bodyMass">Body Mass (kg): </label>
    <input type="number" id="bodyMass" step="0.1" required><br>

    <label for="height">Height (meters): </label>
    <input type="number" id="height" step="0.01" required><br>

    <label for="chairHeight">Chair Height (meters): </label>
    <input type="number" id="chairHeight" step="0.01" required><br>

    <label for="timeFor5STS">Time for 5 STS (seconds): </label>
    <input type="number" id="timeFor5STS" step="0.01" required><br>

    <button type="button" onclick="calculateSitToStandPower()">Calculate</button>
</form>

<div id="result"></div>

<div id="referenceTableContainer">
    <button onclick="toggleTableVisibility()">Toggle Reference Table</button>
    <table id="referenceTable">
        <thead>
            <tr>
                <th>Age Group (Sex)</th>
                <th>3rd Percentile</th>
                <th>10th Percentile</th>
                <th>25th Percentile</th>
                <th>50th Percentile</th>
                <th>75th Percentile</th>
                <th>90th Percentile</th>
                <th>97th Percentile</th>
            </tr>
        </thead>
        <tbody>
          <tr>
            <td>60–64 (M)</td>
            <td>1.24</td>
            <td>1.58</td>
            <td>1.96</td>
            <td>2.38</td>
            <td>2.82</td>
            <td>3.31</td>
            <td>3.82</td>
        </tr>
    <tr>
        <td>60–64 (F)</td>
        <td>0.93</td>
        <td>1.19</td>
        <td>1.47</td>
        <td>1.79</td>
        <td>2.13</td>
        <td>2.50</td>
        <td>2.90</td>
    </tr>
    <tr>
        <td>65–69 (M)</td>
        <td>1.04</td>
        <td>1.39</td>
        <td>1.78</td>
        <td>2.20</td>
        <td>2.66</td>
        <td>3.15</td>
        <td>3.66</td>
    </tr>
    <tr>
        <td>65–69 (F)</td>
        <td>0.87</td>
        <td>1.10</td>
        <td>1.37</td>
        <td>1.67</td>
        <td>2.02</td>
        <td>2.41</td>
        <td>2.84</td>
    </tr>
    <tr>
        <td>70–74 (M)</td>
        <td>0.84</td>
        <td>1.16</td>
        <td>1.52</td>
        <td>1.91</td>
        <td>2.35</td>
        <td>2.82</td>
        <td>3.32</td>
    </tr>
    <tr>
        <td>70–74 (F)</td>
        <td>0.79</td>
        <td>1.01</td>
        <td>1.26</td>
        <td>1.56</td>
        <td>1.90</td>
        <td>2.29</td>
        <td>2.73</td>
    </tr>
    <tr>
        <td>75–79 (M)</td>
        <td>0.72</td>
        <td>0.99</td>
        <td>1.30</td>
        <td>1.67</td>
        <td>2.08</td>
        <td>2.55</td>
        <td>3.07</td>
    </tr>
    <tr>
        <td>75–79 (F)</td>
        <td>0.71</td>
        <td>0.92</td>
        <td>1.16</td>
        <td>1.45</td>
        <td>1.78</td>
        <td>2.17</td>
        <td>2.61</td>
    </tr>
    <tr>
        <td>80–84 (M)</td>
        <td>0.65</td>
        <td>0.88</td>
        <td>1.17</td>
        <td>1.51</td>
        <td>1.92</td>
        <td>2.41</td>
        <td>2.98</td>
    </tr>
    <tr>
        <td>80–84 (F)</td>
        <td>0.64</td>
        <td>0.83</td>
        <td>1.06</td>
        <td>1.33</td>
        <td>1.65</td>
        <td>2.03</td>
        <td>2.46</td>
    </tr>
    <tr>
        <td>85+ (M)</td>
        <td>0.57</td>
        <td>0.75</td>
        <td>0.99</td>
        <td>1.30</td>
        <td>1.72</td>
        <td>2.29</td>
        <td>3.06</td>
    </tr>
    <tr>
        <td>85+ (F)</td>
        <td>0.55</td>
        <td>0.73</td>
        <td>0.95</td>
        <td>1.21</td>
        <td>1.53</td>
        <td>1.91</td>
        <td>2.35</td>
    </tr>
        </tbody>
    </table>
</div>

<canvas id="lineChart"></canvas>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
    function calculateSitToStandPower() {
        var bodyMass = parseFloat(document.getElementById('bodyMass').value);
        var height = parseFloat(document.getElementById('height').value);
        var chairHeight = parseFloat(document.getElementById('chairHeight').value);
        var timeFor5STS = parseFloat(document.getElementById('timeFor5STS').value);

        // Calculate Sit to Stand Power
        var sitToStandPowerResult = (
            (bodyMass * 0.9 * 9.81 * (height * 0.5 - chairHeight)) / (timeFor5STS * 0.1)
        ) / bodyMass;

        // Display the result
        var resultElement = document.getElementById('result');
        resultElement.innerHTML = `<p>Sit to Stand Power: ${sitToStandPowerResult.toFixed(4)} Watts/Kg</p>`;
    }

    function toggleTableVisibility() {
        var table = document.getElementById('referenceTable');
        table.style.display = table.style.display === 'none' ? 'table' : 'none';
    }
    var ctx = document.getElementById('lineChart').getContext('2d');
    var myLineChart = new Chart(ctx, {
        type: 'line',
        data: lineChartData,
        options: {
            responsive: true,
            maintainAspectRatio: false,
            scales: {
                x: {
                    type: 'linear',
                    position: 'bottom',
                    title: {
                        display: true,
                        text: 'Age Group',
                    },
                },
                y: {
                    type: 'linear',
                    position: 'left',
                    title: {
                        display: true,
                        text: 'Sit to Stand Power (Watts/Kg)',
                    },
                },
            },
        },
    });
</script>

</body>
</html>
