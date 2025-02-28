<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora de Cotización de Prendas</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f4f4f4;
            color: #333;
            margin: 0;
            padding: 20px;
        }
        .container {
            max-width: 600px;
            margin: 0 auto;
            background: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        h1 {
            text-align: center;
            color: #000;
        }
        .form-group {
            margin-bottom: 15px;
            border: 1px solid #ddd;
            padding: 10px;
            border-radius: 5px;
            position: relative;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        label {
            display: block;
            margin-bottom: 5px;
        }
        select, input[type="number"], button {
            padding: 10px;
            border-radius: 5px;
            border: 1px solid #ddd;
        }
        select, input[type="number"] {
            flex: 1;
        }
        button {
            background-color: #5cb85c;
            color: white;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #4cae4c;
        }
        .delete-item {
            background-color: #d9534f;
            padding: 10px;
            border-radius: 5px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            width: 40px;
            height: 40px;
        }
        .delete-item:hover {
            background-color: #c9302c;
        }
        table {
            width: 100%;
            margin-top: 20px;
            border-collapse: collapse;
        }
        table, th, td {
            border: 1px solid #ddd;
        }
        th, td {
            padding: 10px;
            text-align: left;
        }
        th {
            background-color: #f8f8f8;
        }
        .total {
            font-weight: bold;
            margin-top: 20px;
        }
        .message {
            margin-top: 20px;
            text-align: center;
            font-style: italic;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Calculadora de Cotización</h1>
        <div id="items">
            <!-- Los grupos de prendas se agregarán aquí dinámicamente -->
        </div>
        <button onclick="addItem()">Agregar prenda</button>
        <button onclick="clearAllItems()" style="background-color: #d9534f;">Eliminar todas las prendas</button>
        <table id="summary">
            <thead>
                <tr>
                    <th>Prenda</th>
                    <th>Cantidad</th>
                    <th>Precio Unitario</th>
                    <th>Subtotal</th>
                    <th>Acción</th>
                </tr>
            </thead>
            <tbody>
                <!-- Los ítems se agregarán aquí dinámicamente -->
            </tbody>
        </table>
        <div class="total">Total General: $<span id="totalAmount">0.00</span></div>
        <div class="total">Total General + Tax (7%): $<span id="totalWithTax">0.00</span></div>
        <div class="message">¿Quieres confirmar tu pedido o tienes dudas? ¡Contáctanos y te ayudamos con tu cotización!</div>
    </div>

    <script>
        let items = [];

        // Función para agregar una nueva prenda
        function addItem() {
            const newGroup = document.createElement('div');
            newGroup.className = 'form-group';
            newGroup.innerHTML = `
                <select class="garment-select">
                    <option value="16.25,15.34,14.63,13.46">T-shirt Shaka Soft / Gildan Hammer</option>
                    <option value="14.95,14.04,13.33,12.16">T-shirt Heavy Cotton</option>
                    <option value="14.95,14.04,13.33,12.16">T-shirt Hanes</option>
                    <option value="23.40,22.75,21.84,20.67">T-shirt Shaka Muy Oversize</option>
                    <option value="28.34,26.91,25.35,24.44">Hoodie Soft</option>
                    <option value="17.16,18.00,18.00,15.34">Gorra Camionero</option>
                    <option value="13.00,12.35,11.44,10.79">Tote Bag</option>
                    <option value="23.66,23.01,22.23,21.84">Polo Gildan</option>
                </select>
                <input type="number" class="quantity-input" min="1" placeholder="Cantidad">
                <div class="delete-item" onclick="deleteItem(this)">🗑️</div>
            `;
            document.getElementById('items').appendChild(newGroup);
        }

        // Función para eliminar una prenda específica
        function deleteItem(button) {
            const itemGroup = button.closest('.form-group');
            itemGroup.remove();
            updateSummary();
        }

        // Función para eliminar todas las prendas
        function clearAllItems() {
            document.getElementById('items').innerHTML = '';
            items = [];
            updateSummary();
        }

        // Función para calcular y actualizar el resumen
        function updateSummary() {
            const summaryBody = document.querySelector('#summary tbody');
            summaryBody.innerHTML = '';
            let totalAmount = 0;

            // Recopilar todos los ítems
            items = [];
            const garmentGroups = document.querySelectorAll('.form-group');
            garmentGroups.forEach(group => {
                const garmentSelect = group.querySelector('.garment-select');
                const quantityInput = group.querySelector('.quantity-input');
                const garmentName = garmentSelect.options[garmentSelect.selectedIndex].text;
                const prices = garmentSelect.value.split(',').map(Number);
                const quantity = parseInt(quantityInput.value);

                if (quantity > 0) {
                    const price = getPrice(prices, quantity);
                    const subtotal = quantity * price;
                    totalAmount += subtotal;

                    items.push({ garmentName, quantity, price, subtotal });

                    const row = document.createElement('tr');
                    row.innerHTML = `
                        <td>${garmentName}</td>
                        <td>${quantity}</td>
                        <td>$${price.toFixed(2)}</td>
                        <td>$${subtotal.toFixed(2)}</td>
                        <td><button onclick="deleteItemFromSummary(this)" style="background-color: #d9534f; padding: 5px 10px;">Eliminar</button></td>
                    `;
                    summaryBody.appendChild(row);
                }
            });

            // Mostrar el total general
            document.getElementById('totalAmount').textContent = totalAmount.toFixed(2);

            // Calcular y mostrar el total con impuesto (7%)
            const taxRate = 0.07;
            const totalWithTax = totalAmount * (1 + taxRate);
            document.getElementById('totalWithTax').textContent = totalWithTax.toFixed(2);
        }

        // Función para eliminar una prenda del resumen
        function deleteItemFromSummary(button) {
            const row = button.closest('tr');
            const index = Array.from(row.parentNode.children).indexOf(row);
            items.splice(index, 1);
            updateSummary();
        }

        // Función para obtener el precio según la cantidad
        function getPrice(prices, quantity) {
            if (quantity >= 1 && quantity <= 20) return prices[0];
            if (quantity >= 21 && quantity <= 50) return prices[1];
            if (quantity >= 51 && quantity <= 100) return prices[2];
            return prices[3];
        }

        // Actualizar el resumen cada vez que se cambia una cantidad o se selecciona una prenda
        document.getElementById('items').addEventListener('input', updateSummary);
    </script>
</body>
</html>
