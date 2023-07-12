# Calculadora-simples-de-investimento-
Calculadora simples de investimento 
<!DOCTYPE html>
<html>
<head>
    <title>Calculadora de Investimento</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 20px;
            background-color: #f7f7f7;
        }
        
        h1 {
            color: #ff7272;
            font-size: 28px;
        }
        
        label {
            display: block;
            margin-top: 10px;
            font-weight: bold;
            color: #666;
        }
        
        input[type="text"],
        select {
            padding: 8px;
            width: 200px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        
        input[type="submit"],
        input[type="button"] {
            padding: 10px 20px;
            margin-top: 20px;
            background-color: #ff7272;
            color: #fff;
            border: none;
            cursor: pointer;
            border-radius: 4px;
        }
        
        #investment-form {
            margin-bottom: 20px;
        }
        
        #result {
            text-align: center;
            background-color: #fff;
            border: 1px solid #ddd;
            border-radius: 4px;
            padding: 20px;
        }
        
        #result table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        
        #result table th,
        #result table td {
            padding: 10px;
            border: 1px solid #ddd;
            color: #333;
        }
        
        #result table th {
            background-color: #f2f2f2;
            font-weight: bold;
        }
        
        .yearly-total {
            background-color: #ffe0e0;
            font-weight: bold;
        }
        
        .number-red {
            color: #ff7272;
        }
        
        .number-blue {
            color: #5386e4;
        }
        
        .number-green {
            color: #43b581;
        }
        
        .number-purple {
            color: #9c60ff;
        }
    </style>
</head>
<body>
    <h1>Calculadora de Investimento</h1>
    <div id="investment-form">
        <form>
            <label for="initial-investment">Investimento Inicial:</label>
            <input type="text" id="initial-investment" placeholder="Valor em Reais">
            
            <label for="investment-per-month">Valor Investido por Mês:</label>
            <input type="text" id="investment-per-month" placeholder="Valor em Reais">
            
            <label for="interest-rate">Taxa de Juros/Retorno (%):</label>
            <input type="text" id="interest-rate" placeholder="Porcentagem">
            
            <label for="start-month">Mês de Início:</label>
            <select id="start-month">
                <option value="0">Janeiro</option>
                <option value="1">Fevereiro</option>
                <option value="2">Março</option>
                <option value="3">Abril</option>
                <option value="4">Maio</option>
                <option value="5">Junho</option>
                <option value="6">Julho</option>
                <option value="7">Agosto</option>
                <option value="8">Setembro</option>
                <option value="9">Outubro</option>
                <option value="10">Novembro</option>
                <option value="11">Dezembro</option>
            </select>
            
            <label for="investment-period">Duração (em anos):</label>
            <input type="text" id="investment-period" placeholder="Anos">
            
            <input type="submit" value="Calcular">
            <input type="button" value="Limpar" onclick="clearFields()">
        </form>
    </div>
    
    <div id="result"></div>
    
    <script>
        function clearFields() {
            document.getElementById('initial-investment').value = '';
            document.getElementById('investment-per-month').value = '';
            document.getElementById('interest-rate').value = '';
            document.getElementById('start-month').value = new Date().getMonth().toString();
            document.getElementById('investment-period').value = '';
            document.getElementById('result').innerHTML = '';
        }
        
        document.querySelector('#investment-form form').addEventListener('submit', function(e) {
            e.preventDefault();
            
            var initialInvestment = parseFloat(document.getElementById('initial-investment').value.replace(",", "."));
            var investmentPerMonth = parseFloat(document.getElementById('investment-per-month').value.replace(",", "."));
            var interestRate = parseFloat(document.getElementById('interest-rate').value.replace(",", "."));
            var startMonth = parseInt(document.getElementById('start-month').value);
            var investmentPeriod = parseFloat(document.getElementById('investment-period').value.replace(",", "."));
            
            if (isNaN(initialInvestment) || isNaN(investmentPerMonth) || isNaN(interestRate) ||
                initialInvestment <= 0 || investmentPerMonth <= 0 || interestRate <= 0 || investmentPeriod <= 0) {
                document.getElementById('result').innerHTML = "<p>Preencha todos os campos corretamente com valores numéricos positivos.</p>";
                return;
            }
            
            var futureValue = initialInvestment;
            var totalInvestment = initialInvestment;
            var result = "<table>";
            result += "<tr><th>Mês</th><th>Total Investido</th><th>Ganhos Mensais</th><th>Valor Total</th></tr>";
            
            var monthNames = [
                "Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho",
                "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"
            ];
            
            var totalEarnings = 0;
            var currentYear = new Date().getFullYear();
            
            for (var i = 1; i <= investmentPeriod * 12; i++) {
                var currentMonth = (i - 1 + startMonth) % 12;
                var monthlyEarnings = futureValue * interestRate / 100 / 12;
                futureValue += monthlyEarnings + investmentPerMonth;
                
                totalInvestment += investmentPerMonth;
                totalEarnings += monthlyEarnings;
                
                var displayMonth = monthNames[currentMonth];
                var displayYear = currentYear + Math.floor((i - 1 + startMonth) / 12);
                
                var numberClass = '';
                if (i % 4 === 0) {
                    numberClass = 'number-red';
                } else if (i % 3 === 0) {
                    numberClass = 'number-blue';
                } else if (i % 2 === 0) {
                    numberClass = 'number-green';
                } else {
                    numberClass = 'number-purple';
                }
                
                result += "<tr><td>" + displayMonth + " " + displayYear + "</td><td class='" + numberClass + "'>R$ " + formatNumber(totalInvestment.toFixed(2)) + "</td><td class='" + numberClass + "'>R$ " + formatNumber(monthlyEarnings.toFixed(2)) + "</td><td class='" + numberClass + "'>R$ " + formatNumber(futureValue.toFixed(2)) + "</td></tr>";
                
                if (i % 12 === 0) {
                    result += "<tr class='yearly-total'><td colspan='4'>Ciclo de 1 ano concluído - Total:</td></tr>";
                    result += "<tr class='yearly-total'><td colspan='2' class='" + numberClass + "'>" + formatNumber(totalInvestment.toFixed(2)) + "</td><td class='" + numberClass + "'>" + formatNumber(totalEarnings.toFixed(2)) + "</td><td class='" + numberClass + "'>" + formatNumber(futureValue.toFixed(2)) + "</td></tr>";
                }
            }
            
            result += "</table>";
            document.getElementById('result').innerHTML = result;
        });

        function formatNumber(number) {
            return number.toLocaleString('pt-BR');
        }
    </script>
</body>
</html>
