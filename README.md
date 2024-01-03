<?php
function calculateElectricityRates($voltage, $current, $currentRate) {
    $power = $voltage * $current;
    $energy = $power * 1 * 1000; 
    $totalCharge = $energy * ($currentRate / 100);

    return [
        'power' => $power,
        'energy' => $energy,
        'totalCharge' => $totalCharge
    ];
}

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $voltage = $_POST['voltage'];
    $current = $_POST['current'];
    $currentRate = $_POST['currentRate'];

    if (!empty($voltage) && is_numeric($voltage) && !empty($current) && is_numeric($current) && !empty($currentRate) && is_numeric($currentRate)) {
        $result = calculateElectricityRates($voltage, $current, $currentRate);
    } else {
        $error = "Please provide valid input values.";
    }
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
    <title>Electricity Calculator</title>
</head>
<body>

<div class="container mt-5">
    <h2>Electricity Calculator</h2>
    <form method="post">
        <div class="form-group">
            <label for="voltage">Voltage (V):</label>
            <input type="text" class="form-control" id="voltage" name="voltage" required>
        </div>
        <div class="form-group">
            <label for="current">Current (A):</label>
            <input type="text" class="form-control" id="current" name="current" required>
        </div>
        <div class="form-group">
            <label for="currentRate">Current Rate (%):</label>
            <input type="text" class="form-control" id="currentRate" name="currentRate" required>
        </div>
        <button type="submit" class="btn btn-primary">Calculate</button>
    </form>

    <?php if (isset($result)): ?>
        <div class="mt-4">
            <h4>Results:</h4>
            <p>Power: <?php echo $result['power']; ?> Watt</p>
            <p>Energy: <?php echo $result['energy']; ?> kWh</p>
            <p>Total Charge: <?php echo $result['totalCharge']; ?></p>
        </div>
    <?php elseif (isset($error)): ?>
        <div class="mt-4 alert alert-danger">
            <?php echo $error; ?>
        </div>
    <?php endif; ?>
</div>

</body>
</html>
