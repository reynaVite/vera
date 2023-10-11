<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <title>Cifrado César</title> 
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
</head>

<body>
    <div class="container mt-5">
        <h1 class="text-center">Cifrado César</h1>
        <div class="form-group">
            <label for="inputText">Texto a cifrar/descifrar:</label>
            <input type="text" id="inputText" class="form-control">
        </div>
        <div class="form-group">
            <label for="shift">Desplazamiento:</label>
            <input type="number" id="shift" class="form-control">
        </div>
        <div class="text-center">
            <button onclick="cifrar()" class="btn btn-primary">Cifrar</button>
            <button onclick="descifrar()" class="btn btn-secondary">Descifrar</button>
        </div>
        <div class="mt-3">
            <p>Texto cifrado/descifrado:</p>
            <p id="outputText" class="border p-2"></p>
        </div>
    </div>

    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.3/dist/umd/popper.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>

    <script>
        function eliminarAcentosDieresis(texto) {
            return texto.normalize("NFD").replace(/[\u0300-\u036f]/g, "");
        }

        function cifrar() {
            realizarCifrado(false);
        }

        function descifrar() {
            realizarCifrado(true);
        }

        function realizarCifrado(descifrar) {
            const textoOriginal = document.getElementById("inputText").value;
            const desplazamiento = parseInt(document.getElementById("shift").value);
            if (isNaN(desplazamiento) || desplazamiento < 1) {
                alert("El desplazamiento debe ser un valor positivo.");
                return;
            }

            const textoSinAcentosDieresis = eliminarAcentosDieresis(textoOriginal);
            let resultado = "";

            for (let i = 0; i < textoSinAcentosDieresis.length; i++) {
                let char = textoSinAcentosDieresis[i];
                if (char.match(/[a-zA-Z]/)) {
                    let charCode = char.charCodeAt(0);
                    let baseCharCode = charCode <= 90 ? 65 : 97;
                    if (descifrar) {
                        char = String.fromCharCode(((charCode - baseCharCode - desplazamiento + 26) % 26) + baseCharCode);
                    } else {
                        char = String.fromCharCode(((charCode - baseCharCode + desplazamiento) % 26) + baseCharCode);
                    }
                }
                resultado += char;
            }

            document.getElementById("outputText").textContent = resultado;
        }
    </script>
</body>

</html>
