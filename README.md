# Aircraft-Calculator
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aircraft Weight and Balance Calculator</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        .input-group { margin-bottom: 10px; }
        label { display: inline-block; width: 150px; }
        input { width: 100px; }
        .results { margin-top: 20px; }
        .warning { color: red; font-weight: bold; }
        .ok { color: green; font-weight: bold; }
        .limits { background-color: #f0f0f0; padding: 10px; margin-bottom: 20px; }
    </style>
</head>
<body>
    <h1>Aircraft Weight and Balance Calculator</h1>
    
    <div class="limits">
        <h2>Aircraft Limits</h2>
        <p>MTOW (Max Takeoff Weight): 3000 lbs</p>
        <p>MLW (Max Landing Weight): 2800 lbs</p>
        <p>MZFW (Max Zero Fuel Weight): 2500 lbs</p>
        <p>CG Range: 35 - 45 inches</p>
    </div>
    
    <h2>Fixed Data (Pre-loaded)</h2>
    <div class="input-group">
        <label>BEW (lbs):</label>
        <span id="bew">1500</span>
    </div>
    <div class="input-group">
        <label>BEW Arm (inches):</label>
        <span id="bew-arm">40</span>
    </div>
    <div class="input-group">
        <label>BEW Moment (inch-lbs):</label>
        <span id="bew-moment">60000</span>
    </div>
    
    <h2>Dynamic Inputs</h2>
    <div class="input-group">
        <label>Payload Weight (lbs):</label>
        <input type="number" id="payload-weight" value="0">
    </div>
    <div class="input-group">
        <label>Payload Arm (inches):</label>
        <input type="number" id="payload-arm" value="42">
    </div>
    <div class="input-group">
        <label>Fuel Weight (lbs):</label>
        <input type="number" id="fuel-weight" value="0">
    </div>
    <div class="input-group">
        <label>Fuel Arm (inches):</label>
        <input type="number" id="fuel-arm" value="42">
    </div>
    
    <h2>Calculations</h2>
    <div class="results">
        <p>Total Weight: <span id="total-weight">0</span> lbs <span id="weight-status"></span></p>
        <p>Total Moment: <span id="total-moment">0</span> inch-lbs</p>
        <p>CG: <span id="cg">0</span> inches <span id="cg-status"></span></p>
    </div>
    
    <script>
        // Fixed values
        const BEW = 1500;
        const BEW_ARM = 40;
        const BEW_MOMENT = 60000;
        
        // Limits
        const MTOW = 3000;
        const MLW = 2800;
        const MZFW = 2500;
        const CG_MIN = 35;
        const CG_MAX = 45;
        
        // Get elements
        const payloadWeight = document.getElementById('payload-weight');
        const payloadArm = document.getElementById('payload-arm');
        const fuelWeight = document.getElementById('fuel-weight');
        const fuelArm = document.getElementById('fuel-arm');
        const totalWeightEl = document.getElementById('total-weight');
        const totalMomentEl = document.getElementById('total-moment');
        const cgEl = document.getElementById('cg');
        const weightStatus = document.getElementById('weight-status');
        const cgStatus = document.getElementById('cg-status');
        
        // Function to calculate
        function calculate() {
            const payloadW = parseFloat(payloadWeight.value) || 0;
            const payloadA = parseFloat(payloadArm.value) || 0;
            const fuelW = parseFloat(fuelWeight.value) || 0;
            const fuelA = parseFloat(fuelArm.value) || 0;
            
            const totalWeight = BEW + payloadW + fuelW;
            const totalMoment = BEW_MOMENT + (payloadW * payloadA) + (fuelW * fuelA);
            const cg = totalMoment / totalWeight;
            
            totalWeightEl.textContent = totalWeight.toFixed(2);
            totalMomentEl.textContent = totalMoment.toFixed(2);
            cgEl.textContent = cg.toFixed(2);
            
            // Check limits
            let weightOk = true;
            if (totalWeight > MTOW) {
                weightStatus.textContent = 'EXCEEDS MTOW!';
                weightStatus.className = 'warning';
                weightOk = false;
            } else if (totalWeight > MLW) {
                weightStatus.textContent = 'Above MLW, check for landing';
                weightStatus.className = 'warning';
            } else if (totalWeight > MZFW) {
                weightStatus.textContent = 'Above MZFW';
                weightStatus.className = 'warning';
            } else {
                weightStatus.textContent = 'OK';
                weightStatus.className = 'ok';
            }
            
            if (cg < CG_MIN || cg > CG_MAX) {
                cgStatus.textContent = 'OUT OF CG RANGE!';
                cgStatus.className = 'warning';
            } else {
                cgStatus.textContent = 'OK';
                cgStatus.className = 'ok';
            }
        }
        
        // Event listeners
        payloadWeight.addEventListener('input', calculate);
        payloadArm.addEventListener('input', calculate);
        fuelWeight.addEventListener('input', calculate);
        fuelArm.addEventListener('input', calculate);
        
        // Initial calculation
        calculate();
    </script>
</body>
</html>
