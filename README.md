<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora de Empréstimo (Daniel)</title>
    <link rel="manifest" href="data:application/json;base64,eyJzY29wZSI6Ii8iLCJuYW1lIjoiQ2FsY3VsYWRvcmEgRW1wcsOpc3RpbW8iLCJkaXNwbGF5Ijoic3RhbmRhbG9uZSIsc3RhcnRfdXJsIjoiLi9pbmRleC5odG1sIiwiaWNvbnMiOlt7InNyYyI6Imljb24ucG5nIiwic2l6ZXMiOiIxOTJ4MTkwIiwidHlwZSI6ImltYWdlL3BuZyJ9XX0=">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #4CAF50 0%, #8BC34A 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: white;
            border-radius: 15px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            max-width: 800px;
            width: 100%;
            padding: 30px;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
        }

        .header h1 {
            color: #4CAF50;
            font-size: 28px;
            margin-bottom: 10px;
        }

        .header p {
            color: #666;
            font-size: 14px;
        }

        .save-indicator {
            text-align: center;
            padding: 8px;
            background: #c8e6c9;
            color: #2e7d32;
            border-radius: 6px;
            margin-bottom: 20px;
            font-size: 13px;
            font-weight: 600;
            display: none;
        }

        .save-indicator.show {
            display: block;
            animation: fadeIn 0.3s;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            color: #333;
            font-weight: 600;
            font-size: 14px;
        }

        input {
            width: 100%;
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 16px;
            transition: border-color 0.3s;
        }

        input:focus {
            outline: none;
            border-color: #4CAF50;
        }

        .input-row {
            display: flex;
            gap: 20px;
        }

        .input-row > .form-group {
            flex: 1;
        }

        .button-group {
            display: flex;
            gap: 10px;
            margin-top: 25px;
        }

        button {
            flex: 1;
            padding: 12px;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
        }

        .btn-add {
            background: linear-gradient(135deg, #4CAF50 0%, #8BC34A 100%);
            color: white;
            width: 100%;
        }

        .btn-add:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(76, 175, 80, 0.3);
        }

        .btn-delete {
            background: #ff9800;
            color: white;
            padding: 6px 12px;
            font-size: 12px;
            cursor: pointer;
            border: none;
            border-radius: 4px;
        }

        .btn-delete:hover {
            background: #f57c00;
        }

        .results {
            margin-top: 30px;
            padding: 20px;
            background: #f9f9f9;
            border-radius: 8px;
            border-left: 4px solid #4CAF50;
            display: none;
        }

        .results.show {
            display: block;
            animation: slideIn 0.3s ease-in-out;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(-10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .result-title {
            color: #4CAF50;
            font-size: 18px;
            font-weight: 700;
            margin-bottom: 15px;
            border-bottom: 2px solid #4CAF50;
            padding-bottom: 10px;
        }

        .error {
            background: #fee;
            color: #c33;
            padding: 15px;
            border-radius: 8px;
            border-left: 4px solid #c33;
            margin-top: 20px;
            display: none;
        }

        .error.show {
            display: block;
        }

        .info-box {
            background: #e8f5e9;
            color: #388E3C;
            padding: 12px;
            border-radius: 6px;
            font-size: 13px;
            margin-top: 15px;
            border-left: 4px solid #4CAF50;
        }

        .table-container {
            max-height: 400px;
            overflow-y: auto;
            margin-top: 20px;
            border: 1px solid #ddd;
            border-radius: 8px;
        }

        .amortization-table {
            width: 100%;
            border-collapse: collapse;
            font-size: 12px;
        }

        .amortization-table th,
        .amortization-table td {
            padding: 8px;
            text-align: right;
            border-bottom: 1px solid #eee;
        }

        .amortization-table th {
            background: #4CAF50;
            color: white;
            font-weight: 700;
            position: sticky;
            top: 0;
        }

        .amortization-table tr:nth-child(even) {
            background: #f9f9f9;
        }

        .amortization-table td:first-child,
        .amortization-table th:first-child {
            text-align: center;
        }

        .saldo-final {
            font-weight: 700;
            background: #c8e6c9 !important;
        }

        @media (max-width: 600px) {
            .input-row {
                flex-direction: column;
                gap: 0;
            }

            .amortization-table th,
            .amortization-table td {
                padding: 6px;
                font-size: 10px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>💰 Simulador de Empréstimo (Daniel)</h1>
            <p>Simule pagamentos variáveis e juros compostos diários</p>
        </div>

        <div class="save-indicator" id="saveIndicator">
            ✅ Dados salvos automaticamente
        </div>

        <form id="initialForm">
            <div class="input-row">
                <div class="form-group">
                    <label for="initialValue">Valor do Empréstimo (R$):</label>
                    <input type="number" id="initialValue" placeholder="" step="0.01" min="0" required>
                </div>

                <div class="form-group">
                    <label for="monthlyRate">Taxa de Juros Mensal (%):</label>
                    <input type="number" id="monthlyRate" placeholder="" step="0.01" min="0" required>
                </div>
            </div>

            <div class="form-group">
                <label for="startDate">Data de Início (Data do Empréstimo):</label>
                <input type="date" id="startDate" required>
            </div>
        </form>

        <div class="info-box">
            💡 Adicione os pagamentos abaixo. Os dados são salvos automaticamente no seu navegador.
        </div>

        <form id="paymentForm">
            <div class="input-row">
                <div class="form-group">
                    <label for="paymentDate">Data do Pagamento:</label>
                    <input type="date" id="paymentDate" required>
                </div>

                <div class="form-group">
                    <label for="paymentValue">Valor da Prestação (R$):</label>
                    <input type="number" id="paymentValue" placeholder="" step="0.01" min="0" required>
                </div>
            </div>

            <div class="button-group">
                <button type="button" class="btn-add" id="btnAddPayment">Adicionar Pagamento</button>
            </div>
        </form>

        <div class="error" id="errorBox"></div>

        <div class="results" id="results">
            <div class="result-title">Tabela de Amortização</div>
            
            <div class="table-container">
                <table class="amortization-table">
                    <thead>
                        <tr>
                            <th>#</th>
                            <th>Data Pag.</th>
                            <th>Dias</th>
                            <th>Prestação</th>
                            <th>Juros</th>
                            <th>Amortização</th>
                            <th>Saldo Devedor</th>
                            <th>Ação</th>
                        </tr>
                    </thead>
                    <tbody id="amortizationBody">
                    </tbody>
                </table>
            </div>

            <div class="info-box" id="summaryBox"></div>
        </div>
    </div>

    <script>
        let payments = [];
        let initialBalance = 0;
        let monthlyRate = 0;
        let startDate = null;
        let lastDate = null;
        let currentBalance = 0;
        let totalInterestPaid = 0;
        let totalPrincipalPaid = 0;

        const formatter = new Intl.NumberFormat('pt-BR', {
            style: 'currency',
            currency: 'BRL',
        });

        function getDaysBetween(date1, date2) {
            const diffTime = Math.abs(date2.getTime() - date1.getTime());
            const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
            return diffDays;
        }

        function showSaveIndicator() {
            const indicator = document.getElementById('saveIndicator');
            indicator.classList.add('show');
            setTimeout(() => {
                indicator.classList.remove('show');
            }, 2000);
        }

        function saveData() {
            const data = {
                payments: payments,
                initialBalance: initialBalance,
                monthlyRate: monthlyRate,
                startDate: startDate ? startDate.toISOString() : null,
                initialValue: document.getElementById('initialValue').value,
                monthlyRateInput: document.getElementById('monthlyRate').value,
                startDateInput: document.getElementById('startDate').value
            };
            localStorage.setItem('loanCalculatorData', JSON.stringify(data));
            showSaveIndicator();
        }

        function loadData() {
            const savedData = localStorage.getItem('loanCalculatorData');
            if (savedData) {
                const data = JSON.parse(savedData);
                
                if (data.initialValue) document.getElementById('initialValue').value = data.initialValue;
                if (data.monthlyRateInput) document.getElementById('monthlyRate').value = data.monthlyRateInput;
                if (data.startDateInput) document.getElementById('startDate').value = data.startDateInput;
                
                payments = data.payments || [];
                initialBalance = data.initialBalance || 0;
                monthlyRate = data.monthlyRate || 0;
                startDate = data.startDate ? new Date(data.startDate) : null;
                
                if (payments.length > 0) {
                    recalculateFromSaved();
                }
            }
        }

        function deletePayment(idx) {
            if (confirm('🗑️ Deseja remover este pagamento?')) {
                payments.splice(idx, 1);
                saveData();
                
                if (payments.length > 0) {
                    recalculateFromSaved();
                } else {
                    document.getElementById('amortizationBody').innerHTML = '';
                    document.getElementById('results').classList.remove('show');
                    
                    initialBalance = 0;
                    monthlyRate = 0;
                    startDate = null;
                    lastDate = null;
                    currentBalance = 0;
                    totalInterestPaid = 0;
                    totalPrincipalPaid = 0;
                }
            }
        }

        function recalculateFromSaved() {
            lastDate = startDate;
            currentBalance = initialBalance;
            totalInterestPaid = 0;
            totalPrincipalPaid = 0;

            const tbody = document.getElementById('amortizationBody');
            tbody.innerHTML = '';

            const initialRow = document.createElement('tr');
            initialRow.innerHTML = `
                <td>0</td>
                <td>${startDate.toLocaleDateString('pt-BR')}</td>
                <td>-</td>
                <td>-</td>
                <td>-</td>
                <td>-</td>
                <td class="saldo-final">${formatter.format(initialBalance)}</td>
                <td>-</td>
            `;
            tbody.appendChild(initialRow);

            payments.forEach((payment, index) => {
                const paymentDate = new Date(payment.date);
                const days = getDaysBetween(lastDate, paymentDate);
                const periodRate = Math.pow(1 + monthlyRate, days / 30) - 1;
                const interest = currentBalance * periodRate;
                const amortization = payment.value - interest;
                const newBalance = currentBalance - amortization;

                totalInterestPaid += interest;
                totalPrincipalPaid += amortization;
                currentBalance = newBalance;
                lastDate = paymentDate;

                payment.days = days;
                payment.interest = interest;
                payment.amortization = amortization;
                payment.balance = newBalance;

                const newRow = document.createElement('tr');
                
                const td1 = document.createElement('td');
                td1.textContent = index + 1;
                
                const td2 = document.createElement('td');
                td2.textContent = paymentDate.toLocaleDateString('pt-BR');
                
                const td3 = document.createElement('td');
                td3.textContent = days;
                
                const td4 = document.createElement('td');
                td4.textContent = formatter.format(payment.value);
                
                const td5 = document.createElement('td');
                td5.textContent = formatter.format(interest);
                
                const td6 = document.createElement('td');
                td6.textContent = formatter.format(amortization);
                
                const td7 = document.createElement('td');
                td7.className = newBalance <= 0 ? 'saldo-final' : '';
                td7.textContent = formatter.format(newBalance);
                
                const td8 = document.createElement('td');
                const deleteBtn = document.createElement('button');
                deleteBtn.className = 'btn-delete';
                deleteBtn.textContent = '❌';
                deleteBtn.dataset.paymentIndex = index;
                td8.appendChild(deleteBtn);
                
                newRow.appendChild(td1);
                newRow.appendChild(td2);
                newRow.appendChild(td3);
                newRow.appendChild(td4);
                newRow.appendChild(td5);
                newRow.appendChild(td6);
                newRow.appendChild(td7);
                newRow.appendChild(td8);
                
                tbody.appendChild(newRow);
            });

            updateSummary();
            document.getElementById('results').classList.add('show');
        }

        function updateSummary() {
            const summaryBox = document.getElementById('summaryBox');
            let status = '';

            if (currentBalance <= 0) {
                status = `<span style="color: green; font-weight: 700;">✅ Empréstimo Quitado!</span>`;
            } else {
                status = `<span style="color: orange; font-weight: 700;">⏳ Saldo Devedor: ${formatter.format(currentBalance)}</span>`;
            }

            summaryBox.innerHTML = `
                ${status}<br>
                Valor Inicial: ${formatter.format(initialBalance)} | Taxa Mensal: ${(monthlyRate * 100).toFixed(2)}%<br>
                Total Pago (Principal): ${formatter.format(totalPrincipalPaid)}<br>
                Total Pago (Juros): ${formatter.format(totalInterestPaid)}
            `;
        }

        function addPayment() {
            const initialValue = parseFloat(document.getElementById('initialValue').value);
            const monthlyRateInput = parseFloat(document.getElementById('monthlyRate').value);
            const startDateInput = document.getElementById('startDate').value;
            const paymentDateInput = document.getElementById('paymentDate').value;
            const paymentValue = parseFloat(document.getElementById('paymentValue').value);

            const errorBox = document.getElementById('errorBox');

            if (isNaN(initialValue) || isNaN(monthlyRateInput) || !startDateInput || isNaN(paymentValue) || !paymentDateInput || initialValue <= 0 || monthlyRateInput < 0 || paymentValue < 0) {
                errorBox.textContent = '❌ Por favor, preencha todos os campos com valores válidos.';
                errorBox.classList.add('show');
                return;
            }

            errorBox.classList.remove('show');

            if (payments.length === 0) {
                initialBalance = initialValue;
                monthlyRate = monthlyRateInput / 100;
                startDate = new Date(startDateInput + 'T00:00:00');
                lastDate = startDate;
                currentBalance = initialBalance;
            }

            const paymentDate = new Date(paymentDateInput + 'T00:00:00');

            if (paymentDate <= lastDate) {
                errorBox.textContent = '❌ A data do pagamento deve ser posterior à data do último pagamento.';
                errorBox.classList.add('show');
                return;
            }

            payments.push({
                date: paymentDate.toISOString(),
                value: paymentValue
            });

            saveData();
            recalculateFromSaved();

            document.getElementById('paymentDate').value = '';
            document.getElementById('paymentValue').value = '';
        }

        document.getElementById('btnAddPayment').addEventListener('click', addPayment);

        document.getElementById('paymentForm').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                e.preventDefault();
                addPayment();
            }
        });

        document.addEventListener('click', function(e) {
            if (e.target && e.target.classList.contains('btn-delete')) {
                const idx = parseInt(e.target.dataset.paymentIndex);
                deletePayment(idx);
            }
        });

        window.addEventListener('load', loadData);
    </script>
</body>
</html>
