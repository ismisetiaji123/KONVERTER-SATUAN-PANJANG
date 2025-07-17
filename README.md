<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>KONVERTER SATUAN PANJANG</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom styles for the converter */
        body {
            font-family: 'Inter', sans-serif;
            /* A more neutral, slightly textured background to mimic a desk or surface */
            background: linear-gradient(135deg, #e0e7ff 0%, #c7d2fe 100%); /* Light blue-purple gradient */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
            color: #334155;
        }
        .converter-container {
            background-color: #f0f0f0; /* Light gray for calculator body */
            padding: 30px;
            border-radius: 25px; /* Even more rounded corners */
            /* Enhanced box-shadow to give a layered, calculator-like feel */
            box-shadow:
                0 25px 50px rgba(0, 0, 0, 0.4), /* Main, deeper shadow for lift */
                inset 0 0 0 4px rgba(255, 255, 255, 0.7), /* Inner white bezel */
                inset 0 0 0 8px rgba(0, 0, 0, 0.1), /* Inner dark border */
                0 0 0 12px rgba(0, 0, 0, 0.03); /* Subtle outer glow */
            text-align: center;
            width: 100%;
            max-width: 650px; /* Wider container to accommodate more options */
            display: flex;
            flex-direction: column;
            gap: 25px; /* More space between elements */
            border: 1px solid #d1d5db; /* Subtle outer border */
            position: relative; /* For potential future elements */
            overflow: hidden; /* Ensure shadows/borders are contained */
        }
        h1 {
            font-size: 2.8rem; /* Slightly larger title */
            font-weight: extra-bold;
            color: #1e293b;
            margin-bottom: 15px; /* More space below title */
            text-shadow: 2px 2px 4px rgba(0,0,0,0.1); /* More pronounced text shadow */
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px; /* Space between text and ruler emoji */
        }
        .ruler-icon {
            font-size: 2.5rem; /* Size for the ruler emoji */
            line-height: 1;
            transform: rotate(45deg); /* Rotate ruler for visual interest */
            display: inline-block;
            animation: bounce 2s infinite ease-in-out; /* Subtle bounce animation */
        }

        @keyframes bounce {
            0%, 100% { transform: rotate(45deg) translateY(0); }
            50% { transform: rotate(45deg) translateY(-5px); }
        }

        .input-group {
            display: flex;
            flex-direction: column;
            gap: 15px;
            align-items: center;
        }
        .input-row {
            display: flex;
            align-items: center;
            gap: 15px;
            width: 100%;
            justify-content: center;
            flex-wrap: wrap; /* Allow wrapping on smaller screens */
        }
        input[type="number"], select {
            padding: 12px 15px;
            border: 2px solid #cbd5e1;
            border-radius: 10px;
            font-size: 1.1rem;
            text-align: center;
            outline: none;
            color: #1e293b;
            box-shadow: inset 0 2px 5px rgba(0,0,0,0.05);
            flex-grow: 1; /* Allow input/select to grow */
            min-width: 120px; /* Minimum width for inputs/selects */
            transition: border-color 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
            background-color: #ffffff; /* Ensure input background is white */
        }
        input[type="number"]:focus, select:focus {
            border-color: #6366f1;
            box-shadow: 0 0 0 4px rgba(99, 102, 241, 0.4); /* More prominent focus glow */
        }
        button {
            background-color: #6366f1;
            color: white;
            padding: 15px 30px;
            border-radius: 12px;
            font-size: 1.2rem;
            font-weight: bold;
            cursor: pointer;
            transition: background-color 0.2s ease-in-out, transform 0.1s ease-in-out, box-shadow 0.2s ease-in-out;
            border: none;
            box-shadow: 0 6px 15px rgba(99, 102, 241, 0.4);
            letter-spacing: 0.5px;
            width: fit-content; /* Adjust button width to content */
            margin-top: 10px;
        }
        button:hover {
            background-color: #4f46e5;
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(99, 102, 241, 0.5);
        }
        button:active {
            transform: translateY(0);
            box-shadow: 0 3px 8px rgba(99, 102, 241, 0.3);
        }
        .result-display {
            background-color: #e2e8f0; /* Slightly darker gray for result screen */
            padding: 20px;
            border-radius: 15px;
            font-size: 1.8rem;
            font-weight: bold;
            color: #1e293b;
            min-height: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
            word-break: break-all; /* Break long words */
            box-shadow: inset 0 2px 5px rgba(0,0,0,0.05); /* Inner shadow for screen depth */
            animation: fadeIn 0.5s ease-out; /* Fade in animation for result */
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .error-message {
            color: #ef4444;
            font-size: 1rem;
            margin-top: -15px;
            margin-bottom: 10px;
            min-height: 20px;
        }

        /* Responsive adjustments */
        @media (max-width: 600px) {
            .converter-container {
                padding: 20px;
                gap: 20px;
            }
            h1 {
                font-size: 2rem;
            }
            .ruler-icon {
                font-size: 1.8rem;
            }
            .input-row {
                flex-direction: column;
                gap: 10px;
            }
            input[type="number"], select {
                width: 100%;
                max-width: 250px; /* Limit width on small screens */
            }
            .result-display {
                font-size: 1.5rem;
                padding: 15px;
            }
            button {
                width: 100%;
            }
        }
    </style>
</head>
<body>
    <div class="converter-container">
        <h1>KONVERTER SATUAN PANJANG <span class="ruler-icon">📏</span></h1>

        <div class="input-group">
            <div class="input-row">
                <input type="number" id="inputValue" placeholder="Masukkan nilai" step="any">
                <select id="fromUnit" class="p-2 border rounded">
                    <option value="km">Kilometer (km)</option>
                    <option value="hm">Hektometer (hm)</option>
                    <option value="dam">Dekameter (dam)</option>
                    <option value="m" selected>Meter (m)</option>
                    <option value="dm">Desimeter (dm)</option>
                    <option value="cm">Sentimeter (cm)</option>
                    <option value="mm">Milimeter (mm)</option>
                    <option value="um">Mikrometer (µm)</option>
                    <option value="in">Inci (in)</option>
                    <option value="ft">Kaki (ft)</option>
                    <option value="yd">Yard (yd)</option>
                    <option value="mi">Mil (mil)</option>
                    <option value="hand">Hand (hand)</option>
                    <option value="cable">Cable (cable)</option>
                    <option value="au">Satuan Astronomi (SA)</option>
                    <option value="ly">Tahun Cahaya (tc)</option>
                </select>
                <span>ke</span>
                <select id="toUnit" class="p-2 border rounded">
                    <option value="km">Kilometer (km)</option>
                    <option value="hm">Hektometer (hm)</option>
                    <option value="dam">Dekameter (dam)</option>
                    <option value="m">Meter (m)</option>
                    <option value="dm" selected>Desimeter (dm)</option>
                    <option value="cm">Sentimeter (cm)</option>
                    <option value="mm">Milimeter (mm)</option>
                    <option value="um">Mikrometer (µm)</option>
                    <option value="in">Inci (in)</option>
                    <option value="ft">Kaki (ft)</option>
                    <option value="yd">Yard (yd)</option>
                    <option value="mi">Mil (mil)</option>
                    <option value="hand">Hand (hand)</option>
                    <option value="cable">Cable (cable)</option>
                    <option value="au">Satuan Astronomi (SA)</option>
                    <option value="ly">Tahun Cahaya (tc)</option>
                </select>
            </div>
            <div id="errorMessage" class="error-message"></div>
            <button id="convertButton">Konversi</button>
        </div>

        <div id="resultDisplay" class="result-display">
            Hasil Konversi Akan Muncul Di Sini
        </div>
    </div>

    <script type="module">
        // Get references to HTML elements
        const inputValue = document.getElementById('inputValue');
        const fromUnit = document.getElementById('fromUnit');
        const toUnit = document.getElementById('toUnit');
        const convertButton = document.getElementById('convertButton');
        const resultDisplay = document.getElementById('resultDisplay');
        const errorMessage = document.getElementById('errorMessage');

        // Conversion factors relative to meters (m)
        const conversionFactors = {
            'km': 1000,             // Kilometer
            'hm': 100,              // Hektometer
            'dam': 10,              // Dekameter
            'm': 1,                 // Meter
            'dm': 0.1,              // Desimeter
            'cm': 0.01,             // Sentimeter
            'mm': 0.001,            // Milimeter
            'um': 0.000001,         // Mikrometer (10^-6 m)
            'in': 0.0254,           // Inci (1 in = 0.0254 m)
            'ft': 0.3048,           // Kaki (1 ft = 0.3048 m)
            'yd': 0.9144,           // Yard (1 yd = 0.9144 m)
            'mi': 1609.34,          // Mil (1 mil = 1609.34 m)
            'hand': 0.1016,         // Hand (1 hand = 4 inci = 0.1016 m)
            'cable': 185.2,         // Cable (1 cable = 1/10 mil laut = 185.2 m)
            'au': 149597870700,      // Satuan Astronomi (1 AU = 149,597,870,700 m)
            'ly': 9460730472580800   // Tahun Cahaya (1 tc = 9,460,730,472,580,800 m)
        };

        /**
         * Performs the length unit conversion based on user input.
         */
        function convertLength() {
            const value = parseFloat(inputValue.value); // Get the numerical value from input
            const from = fromUnit.value; // Get the unit to convert from
            const to = toUnit.value;     // Get the unit to convert to

            // Clear previous error message
            errorMessage.textContent = '';

            // Validate input
            if (isNaN(value)) {
                errorMessage.textContent = 'Masukkan nilai angka yang valid!';
                resultDisplay.textContent = 'Masukkan nilai yang valid';
                return;
            }

            // Perform conversion
            // Step 1: Convert the input value to meters
            const valueInMeters = value * conversionFactors[from];

            // Step 2: Convert the value in meters to the target unit
            const convertedValue = valueInMeters / conversionFactors[to];

            // Display the result, formatted to avoid excessive decimal places
            // Use toPrecision for very large or very small numbers to maintain readability
            let formattedValue;
            if (Math.abs(convertedValue) > 1e6 || Math.abs(convertedValue) < 1e-6) {
                // Use toFixed for small numbers to avoid scientific notation unless truly necessary
                // For numbers between 1e-6 and 1e6, toFixed(6) is usually good.
                // For numbers outside this range, toExponential is more appropriate.
                if (convertedValue === 0) { // Handle exact zero to avoid -0.000000e+0
                    formattedValue = '0';
                } else {
                    formattedValue = convertedValue.toExponential(6); // Use scientific notation for very large/small numbers
                }
            } else {
                formattedValue = convertedValue.toFixed(6).replace(/\.?0+$/, ''); // Limit decimals, remove trailing zeros
            }

            resultDisplay.textContent = `${value} ${from} = ${formattedValue} ${to}`;
        }

        // Event listener for the convert button
        convertButton.addEventListener('click', convertLength);

        // Optional: Perform conversion on Enter key press in the input field
        inputValue.addEventListener('keypress', function(event) {
            if (event.key === 'Enter' || event.keyCode === 13) {
                convertLength();
            }
        });

        // Optional: Perform conversion when units are changed (for quicker interaction)
        fromUnit.addEventListener('change', convertLength);
        toUnit.addEventListener('change', convertLength);

        // Initial display when the page loads
        document.addEventListener('DOMContentLoaded', () => {
            resultDisplay.textContent = 'Masukkan nilai dan pilih unit untuk konversi.';
        });
    </script>
</body>
</html>
