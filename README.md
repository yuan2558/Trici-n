<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora Nutricional de Porciones</title>
    <style>
        :root {
            --primary: #2563eb;
            --primary-dark: #1d4ed8;
            --success: #16a34a;
            --background: #f8fafc;
            --card-bg: #ffffff;
            --text: #1e293b;
            --text-light: #64748b;
            --border: #e2e8f0;
        }
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
        }
        body {
            background-color: var(--background);
            color: var(--text);
            padding: 16px;
            max-width: 600px;
            margin: 0 auto;
            -webkit-tap-highlight-color: transparent;
        }
        h1 {
            font-size: 1.35rem;
            text-align: center;
            margin-bottom: 4px;
            color: var(--primary);
        }
        .subtitle {
            text-align: center;
            font-size: 0.85rem;
            color: var(--text-light);
            margin-bottom: 16px;
        }
        .card {
            background: var(--card-bg);
            border-radius: 12px;
            padding: 12px;
            margin-bottom: 12px;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
            border: 1px solid var(--border);
        }
        .food-row {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 10px 0;
            border-bottom: 1px solid var(--border);
        }
        .food-row:last-child {
            border-bottom: none;
        }
        .food-info {
            flex: 1;
            padding-right: 8px;
        }
        .food-name {
            font-weight: 600;
            font-size: 0.95rem;
        }
        .food-reference {
            font-size: 0.72rem;
            color: var(--text-light);
            margin-top: 2px;
        }
        .food-subtotal {
            font-size: 0.75rem;
            color: var(--primary);
            font-weight: 600;
            margin-top: 3px;
        }
        .stepper {
            display: flex;
            align-items: center;
            background: #f1f5f9;
            border-radius: 8px;
            padding: 2px;
        }
        .stepper button {
            background: white;
            border: 1px solid var(--border);
            color: var(--primary);
            width: 32px;
            height: 32px;
            border-radius: 6px;
            font-size: 1.1rem;
            font-weight: bold;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: 0 1px 2px rgba(0,0,0,0.05);
        }
        .stepper button:active {
            background: #e2e8f0;
        }
        .stepper input {
            width: 40px;
            text-align: center;
            border: none;
            background: transparent;
            font-size: 1rem;
            font-weight: 600;
            color: var(--text);
            outline: none;
        }
        input::-webkit-outer-spin-button,
        input::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
        
        .summary-card {
            background: linear-gradient(135deg, #1e40af, #2563eb);
            color: white;
            border-radius: 12px;
            padding: 16px;
            margin-top: 16px;
            box-shadow: 0 4px 6px -1px rgba(37, 99, 235, 0.2);
        }
        .summary-title {
            font-size: 1rem;
            font-weight: 600;
            margin-bottom: 12px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid rgba(255,255,255,0.2);
            padding-bottom: 8px;
        }
        .macros-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 8px;
            text-align: center;
        }
        .macro-box {
            background: rgba(255, 255, 255, 0.1);
            padding: 8px 4px;
            border-radius: 8px;
        }
        .macro-val {
            font-size: 1.1rem;
            font-weight: 700;
        }
        .macro-label {
            font-size: 0.7rem;
            opacity: 0.8;
            margin-top: 2px;
            text-transform: uppercase;
        }

        .target-card {
            background: var(--card-bg);
            border-radius: 12px;
            padding: 16px;
            margin-top: 16px;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
            border: 1px solid var(--border);
        }
        .target-title {
            font-size: 1rem;
            font-weight: 600;
            color: var(--primary);
            margin-bottom: 12px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .target-inputs {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 12px;
        }
        .input-group {
            display: flex;
            flex-direction: column;
        }
        .input-group label {
            font-size: 0.75rem;
            font-weight: 600;
            color: var(--text-light);
            margin-bottom: 4px;
            text-transform: uppercase;
        }
        .input-group input {
            background: #f1f5f9;
            border: 1px solid var(--border);
            border-radius: 8px;
            padding: 8px 10px;
            font-size: 1rem;
            font-weight: 600;
            color: var(--text);
            outline: none;
            width: 100%;
        }
        .input-group input:focus {
            border-color: var(--primary);
            background: white;
        }

        .target-comparison {
            background: #f8fafc;
            border-radius: 8px;
            padding: 10px;
            border: 1px solid var(--border);
            font-size: 0.85rem;
        }
        .comp-header {
            font-weight: 600;
            margin-bottom: 8px;
            color: var(--text);
            text-align: center;
        }
        .comp-row {
            display: flex;
            justify-content: space-between;
            padding: 4px 0;
            border-bottom: 1px dashed var(--border);
        }
        .comp-row:last-child {
            border-bottom: none;
        }
        .in-range {
            color: var(--success);
            font-weight: 600;
        }
        .out-range {
            color: #dc2626;
            font-weight: 600;
        }
        
        .actions {
            margin-top: 16px;
            display: flex;
            gap: 8px;
        }
        .btn {
            flex: 1;
            background: white;
            color: var(--primary);
            border: 1px solid var(--border);
            padding: 10px;
            border-radius: 8px;
            font-weight: 600;
            font-size: 0.9rem;
            cursor: pointer;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
            text-align: center;
        }
        .btn:active {
            background: #f1f5f9;
        }
    </style>
</head>
<body>

    <h1>Calculadora de Porciones</h1>
    <div class="subtitle">Diseñado para celular • Rápido y táctil</div>

    <div class="card" id="food-list">
        <!-- Generado por JavaScript -->
    </div>

    <div class="summary-card">
        <div class="summary-title">
            <span>Total Calorías</span>
            <span id="total-kcal" style="font-size: 1.25rem;">0 kcal</span>
        </div>
        
        <div class="macros-grid">
            <div class="macro-box">
                <div class="macro-val" id="total-prot">0g</div>
                <div class="macro-label">Proteína</div>
            </div>
            <div class="macro-box">
                <div class="macro-val" id="total-gras">0g</div>
                <div class="macro-label">Grasa</div>
            </div>
            <div class="macro-box">
                <div class="macro-val" id="total-chos">0g</div>
                <div class="macro-label">Carbos</div>
            </div>
            <div class="macro-box">
                <div class="macro-val" id="total-porciones">0</div>
                <div class="macro-label">Porciones</div>
            </div>
        </div>
    </div>

    <!-- Metas y rangos configurables -->
    <div class="target-card">
        <div class="target-title">
            <span>Metas Nutricionales (Objetivo)</span>
        </div>
        <div class="target-inputs">
            <div class="input-group">
                <label>Calorías Objetivo</label>
                <input type="number" id="target-kcal" placeholder="Ej. 2000" oninput="calculateTotals()">
            </div>
            <div class="input-group">
                <label>Proteína Objetivo (g)</label>
                <input type="number" id="target-prot" placeholder="Ej. 150" oninput="calculateTotals()">
            </div>
            <div class="input-group">
                <label>Grasa Objetivo (g)</label>
                <input type="number" id="target-gras" placeholder="Ej. 65" oninput="calculateTotals()">
            </div>
            <div class="input-group">
                <label>Carbos Objetivo (g)</label>
                <input type="number" id="target-chos" placeholder="Ej. 200" oninput="calculateTotals()">
            </div>
        </div>

        <div class="target-comparison" id="comparison-box" style="display: none;">
            <div class="comp-header">Comparativa con Margen (±10%)</div>
            <div id="comp-rows"></div>
        </div>
    </div>

    <div class="actions">
        <button class="btn" onclick="resetValues()">Reiniciar Valores</button>
    </div>

    <script>
        const foods = [
            { name: "Leches", kcal: 139, prot: 6.7, gras: 6.7, chos: 13.0, qty: 0 },
            { name: "Sustitutos", kcal: 77, prot: 5.8, gras: 5.5, chos: 1.1, qty: 0 },
            { name: "Carnes Magras", kcal: 108, prot: 19.1, gras: 3.1, chos: 1.0, qty: 0 },
            { name: "Harinas", kcal: 95, prot: 2.0, gras: 0.6, chos: 20.5, qty: 0 },
            { name: "P.Grasas", kcal: 45, prot: 0.3, gras: 4.6, chos: 0.5, qty: 0 },
            { name: "Vegetales", kcal: 30, prot: 1.2, gras: 0.3, chos: 5.5, qty: 0 },
            { name: "Frutas", kcal: 59, prot: 1.0, gras: 0.3, chos: 13.3, qty: 0 },
            { name: "Azucar", kcal: 90, prot: 0.9, gras: 1.1, chos: 19.1, qty: 0 }
        ];

        function renderFoods() {
            const container = document.getElementById('food-list');
            container.innerHTML = '';
            foods.forEach((food, index) => {
                let subKcal = (food.qty * food.kcal).toFixed(0);
                let subProt = (food.qty * food.prot).toFixed(1);
                let subGras = (food.qty * food.gras).toFixed(1);
                let subChos = (food.qty * food.chos).toFixed(1);

                container.innerHTML += `
                    <div class="food-row">
                        <div class="food-info">
                            <div class="food-name">${food.name}</div>
                            <div class="food-reference">Base: ${food.kcal} kcal | P: ${food.prot}g | G: ${food.gras}g | C: ${food.chos}g</div>
                            <div class="food-subtotal" id="sub-${index}">Total: ${subKcal} kcal | P: ${subProt}g | G: ${subGras}g | C: ${subChos}g</div>
                        </div>
                        <div class="stepper">
                            <button onclick="updateQty(${index}, -0.5)">-</button>
                            <input type="number" id="qty-${index}" value="${food.qty}" step="0.5" min="0" onchange="inputChange(${index}, this.value)">
                            <button onclick="updateQty(${index}, 0.5)">+</button>
                        </div>
                    </div>
                `;
            });
            calculateTotals();
        }

        function updateQty(index, delta) {
            foods[index].qty = Math.max(0, parseFloat((foods[index].qty + delta).toFixed(1)));
            document.getElementById(`qty-${index}`).value = foods[index].qty;
            updateRowSubtotal(index);
            calculateTotals();
        }

        function inputChange(index, value) {
            let val = parseFloat(value);
            if (isNaN(val) || val < 0) val = 0;
            foods[index].qty = val;
            updateRowSubtotal(index);
            calculateTotals();
        }

        function updateRowSubtotal(index) {
            const food = foods[index];
            let subKcal = (food.qty * food.kcal).toFixed(0);
            let subProt = (food.qty * food.prot).toFixed(1);
            let subGras = (food.qty * food.gras).toFixed(1);
            let subChos = (food.qty * food.chos).toFixed(1);

            const subEl = document.getElementById(`sub-${index}`);
            if(subEl) {
                subEl.innerText = `Total: ${subKcal} kcal | P: ${subProt}g | G: ${subGras}g | C: ${subChos}g`;
            }
        }

        function calculateTotals() {
            let totalKcal = 0;
            let totalProt = 0;
            let totalGras = 0;
            let totalChos = 0;
            let totalQty = 0;

            foods.forEach((food, index) => {
                updateRowSubtotal(index);
                totalKcal += food.qty * food.kcal;
                totalProt += food.qty * food.prot;
                totalGras += food.qty * food.gras;
                totalChos += food.qty * food.chos;
                totalQty += food.qty;
            });

            document.getElementById('total-kcal').innerText = `${Math.round(totalKcal)} kcal`;
            document.getElementById('total-prot').innerText = `${totalProt.toFixed(1)}g`;
            document.getElementById('total-gras').innerText = `${totalGras.toFixed(1)}g`;
            document.getElementById('total-chos').innerText = `${totalChos.toFixed(1)}g`;
            document.getElementById('total-porciones').innerText = totalQty;

            // Target Evaluation
            const tKcal = parseFloat(document.getElementById('target-kcal').value);
            const tProt = parseFloat(document.getElementById('target-prot').value);
            const tGras = parseFloat(document.getElementById('target-gras').value);
            const tChos = parseFloat(document.getElementById('target-chos').value);

            const compBox = document.getElementById('comparison-box');
            const compRows = document.getElementById('comp-rows');

            let hasTarget = !isNaN(tKcal) || !isNaN(tProt) || !isNaN(tGras) || !isNaN(tChos);

            if (!hasTarget) {
                compBox.style.display = 'none';
                return;
            }

            compBox.style.display = 'block';
            compRows.innerHTML = '';

            if (!isNaN(tKcal)) {
                let min = tKcal * 0.9;
                let max = tKcal * 1.1;
                let ok = totalKcal >= min && totalKcal <= max;
                compRows.innerHTML += `
                    <div class="comp-row">
                        <span>Calorías (Meta: ${tKcal}):</span>
                        <span class="${ok ? 'in-range' : 'out-range'}">${Math.round(totalKcal)} kcal (${Math.round(min)} - ${Math.round(max)})</span>
                    </div>
                `;
            }

            if (!isNaN(tProt)) {
                let min = tProt * 0.9;
                let max = tProt * 1.1;
                let ok = totalProt >= min && totalProt <= max;
                compRows.innerHTML += `
                    <div class="comp-row">
                        <span>Proteína (Meta: ${tProt}g):</span>
                        <span class="${ok ? 'in-range' : 'out-range'}">${totalProt.toFixed(1)}g (${min.toFixed(1)} - ${max.toFixed(1)})</span>
                    </div>
                `;
            }

            if (!isNaN(tGras)) {
                let min = tGras * 0.9;
                let max = tGras * 1.1;
                let ok = totalGras >= min && totalGras <= max;
                compRows.innerHTML += `
                    <div class="comp-row">
                        <span>Grasa (Meta: ${tGras}g):</span>
                        <span class="${ok ? 'in-range' : 'out-range'}">${totalGras.toFixed(1)}g (${min.toFixed(1)} - ${max.toFixed(1)})</span>
                    </div>
                `;
            }

            if (!isNaN(tChos)) {
                let min = tChos * 0.9;
                let max = tChos * 1.1;
                let ok = totalChos >= min && totalChos <= max;
                compRows.innerHTML += `
                    <div class="comp-row">
                        <span>Carbos (Meta: ${tChos}g):</span>
                        <span class="${ok ? 'in-range' : 'out-range'}">${totalChos.toFixed(1)}g (${min.toFixed(1)} - ${max.toFixed(1)})</span>
                    </div>
                `;
            }
        }

        function resetValues() {
            foods.forEach(food => food.qty = 0);
            document.getElementById('target-kcal').value = '';
            document.getElementById('target-prot').value = '';
            document.getElementById('target-gras').value = '';
            document.getElementById('target-chos').value = '';
            renderFoods();
        }

        // Inicializar
        renderFoods();
    </script>
</body>
</html>
