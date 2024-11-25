<html>
<header>
    <div>
    <img src="logo.png" class="illustration">
    <h1>Sitio Buyuan-Baras Coffee Growers Association</h1>
    </div>
</header>
    <style>
        div{
            font-size:0.3rem;
            display:flex;
            align-items: center;
            margin-bottom:1rem;

        }

        header img{
            height:3.5rem;
            width:3.5rem;
        }
        
        body{
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 50vh;
      font-family: "calibri light";

        }
h1, h2 {
            text-align: center;
            font-family: "Arial";
        }

        table {
            width: 60%;
            margin: 40px auto;
            border-collapse: collapse;
            font-family: 'arial';
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Subtle shadow */
        }

        th, td {
            padding: 12px 20px;  /* Larger padding for better readability */
            text-align: left;
            border-bottom: 1px solid #ddd; /* Light gray bottom borders */
        }

        th {
            background-color: #f4f4f4; /* Light gray background for headers */
            color: #333;
            font-weight: bold;
        }

        .month{
            display:flex;
        }
        td:first-child {
            text-align: left; /* Align the 'Item' column to the left */
        }

        tr:hover {
            background-color: #f9f9f9; /* Hover effect for rows */
        }

        tr:nth-child(even) {
            background-color: #f9f9f9; /* Alternate row colors for better readability */
        }

        tr:nth-child(odd) {
            background-color: #ffffff;
        }

        caption {
            font-size: 24px;
            font-weight: bold;
            margin-bottom: 10px;
        }

       
    </style>
    </html>
<?php
// Connect to the database
$connection = mysqli_connect("localhost", "root", "", "coffee_growers");
if (!$connection) {
    die("Connection failed: " . mysqli_connect_error());
}

// Fetch data from the "bought" table
$boughtData = mysqli_query($connection, "SELECT beans_price, quantity FROM bought");

// Fetch data from the "sold" table
$soldData = mysqli_query($connection, "SELECT price, qnty FROM sold");

// Initialize totals
$totalRevenue = 0;
$totalCOGS = 0;

// Calculate total revenue from the "sold" table
while ($row = mysqli_fetch_assoc($soldData)) {
    $totalRevenue += (float) $row['price'] * (float) $row['qnty'];

}

// Calculate total cost of goods sold (COGS) from the "bought" table
while ($row = mysqli_fetch_assoc($boughtData)) {
    $totalCOGS += (float) $row['beans_price'] * (float) $row['quantity'];
}

// Calculate net income
$netIncome = $totalRevenue - $totalCOGS;

// Get current month and year
$currentMonth = date("F");
$currentYear = date("Y");

// Display the income statement
echo "<h1>Income Statement</h1>";
echo "<h2>For the Period of </h2>";
echo '<select name="month" class="month">';
for ($m = 1; $m <= 12; $m++) {
    $monthName = date("F", mktime(0, 0, 0, $m, 1));
    $selected = ($monthName == $currentMonth) ? 'selected' : '';
    echo '<option value="' . $monthName . '" ' . $selected . '>' . $monthName . '</option>';
}
echo '</select>';

// Generate years dropdown (e.g., 2000 to 2030)
echo '<select name="year" class="month">';
for ($y = 2000; $y <= 2030; $y++) {
    $selected = ($y == $currentYear) ? 'selected' : '';
    echo '<option value="' . $y . '" ' . $selected . '>' . $y . '</option>';
}
echo '</select>';
echo "<table border='1' style='width: 50%; border-collapse: collapse;'>";
echo "<tr><th>Item</th><th>Amount (₱)</th></tr>";
echo "<tr><td><b>Revenue</b></td><td></td></tr>";
echo "<tr><td>Total Sales</td><td style='text-align: right;'>₱" . number_format($totalRevenue, 2) . "</td></tr>";
echo "<tr><td><b>Expenses</b></td><td></td></tr>";
echo "<tr><td>Cost of Goods Sold</td><td style='text-align: right;'>₱" . number_format($totalCOGS, 2) . "</td></tr>";
echo "<tr><td><b>Net Income</b></td><td style='text-align: right;'>₱" . number_format($netIncome, 2) . "</td></tr>";
echo "</table>";

// Close the database connection
mysqli_close($connection);
?>

<script>
   function printDiv(){
      printButton.style.visibility = 'hidden';
      window.print();
      printButton.style.visibility = 'visible';
    }
</script>
<div>
<input id="printButton" type="button" value="Print Result" onclick="printDiv()"/>
</div>
</body>
</html>
