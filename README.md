<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Registro Financeiro</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f5f5f5;
            margin: 20px;
        }

        h1, h2 {
            color: #333;
        }

        input, select {
            padding: 8px;
            margin: 5px 0;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-sizing: border-box;
        }

        button {
            padding: 8px 16px;
            border: 1px solid #ccc;
            border-radius: 5px;
            background-color: #007bff;
            color: #fff;
            cursor: pointer;
        }

        button:hover {
            background-color: #0056b3;
        }

        #historico {
            margin-top: 10px;
            margin-bottom: 10px;
        }

        table {
            border-collapse: collapse;
            width: 100%;
        }

        th, td {
            border: 1px solid #ddd;
            padding: 12px;
            text-align: left;
        }

        th {
            background-color: #007bff;
            color: #fff;
        }

        #total {
            font-size: 18px;
            font-weight: bold;
            margin-top: 10px;
            color: #333;
        }

        #pesquisa {
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <h1>Registro Financeiro</h1>
    <h2>Adicionar Transação</h2>
    <p>
        <label for="tipo">Tipo:</label>
        <select id="tipo">
            <option value="entrada">Entrada</option>
            <option value="saida">Saída</option>
        </select>
    </p>
    <p>
        <label for="data">Data:</label>
        <input type="date" id="data">
    </p>
    <p>
        <label for="descricao">Descrição:</label>
        <input type="text" id="descricao">
    </p>
    <p>
        <label for="valor">Valor:</label>
        <input type="number" id="valor" step="0.01">
    </p>
    <p>
        <button id="buttonAdicionar">Adicionar Transação</button>
    </p>

    <h2>Histórico de Transações</h2>
    <div id="pesquisa">
        <label for="filtro">Filtrar por Descrição ou Tipo:</label>
        <input type="text" id="filtro" oninput="filtrarHistorico()">
    </div>
    <table>
        <thead>
            <tr>
                <th>Data</th>
                <th>Descrição</th>
                <th>Tipo</th>
                <th>Valor</th>
            </tr>
        </thead>
        <tbody id="historico"></tbody>
    </table>

    <div id="total">Total: R$ 0.00</div>

    <script>
        const tipo = document.getElementById("tipo");
        const data = document.getElementById("data");
        const descricao = document.getElementById("descricao");
        const valor = document.getElementById("valor");
        const buttonAdicionar = document.getElementById("buttonAdicionar");
        const historico = document.getElementById("historico");
        const totalElement = document.getElementById("total");
        const filtro = document.getElementById("filtro");
        let totalEntradas = 0.0;
        let totalSaidas = 0.0;

        buttonAdicionar.addEventListener("click", () => {
            const tipoSelecionado = tipo.value;
            const dataTransacao = formatarData(data.value);
            const descricaoTransacao = descricao.value;
            const valorTransacao = parseFloat(valor.value);

            if (isNaN(valorTransacao)) {
                alert("Por favor, insira um valor válido.");
                return;
            }

            const row = `<tr>
                            <td>${dataTransacao}</td>
                            <td>${descricaoTransacao}</td>
                            <td>${tipoSelecionado}</td>
                            <td>R$ ${valorTransacao.toFixed(2)}</td>
                        </tr>`;

            historico.innerHTML += row;

            if (tipoSelecionado === "entrada") {
                totalEntradas += valorTransacao;
            } else if (tipoSelecionado === "saida") {
                totalSaidas += valorTransacao;
            }

            atualizarTotal();
            limparCampos();
        });

        function filtrarHistorico() {
            const termoFiltro = filtro.value.toLowerCase();
            const linhas = historico.getElementsByTagName("tr");

            for (let i = 0; i < linhas.length; i++) {
                const descricaoTransacao = linhas[i].getElementsByTagName("td")[1];
                const tipoTransacao = linhas[i].getElementsByTagName("td")[2];

                if (descricaoTransacao && tipoTransacao) {
                    const textoDescricao = descricaoTransacao.textContent.toLowerCase();
                    const textoTipo = tipoTransacao.textContent.toLowerCase();

                    if (textoDescricao.includes(termoFiltro) || textoTipo.includes(termoFiltro)) {
                        linhas[i].style.display = "";
                    } else {
                        linhas[i].style.display = "none";
                    }
                }
            }
        }

        function formatarData(dataIso) {
            const [ano, mes, dia] = dataIso.split("-");
            return `${dia}/${mes}/${ano.substr(2)}`;
        }

        function atualizarTotal() {
            const totalGeral = totalEntradas - totalSaidas;
            totalElement.textContent = `Total: R$ ${totalGeral.toFixed(2)}`;
        }

        function limparCampos() {
            data.value = "";
            descricao.value = "";
            valor.value = "";
        }
    </script>
</body>
</html>
