# columnar-cipher-index.html
<!DOCTYPE html>
<html>

<head>

<title>Columnar Transposition Cipher</title>

<style>

body {
    font-family: Arial;
    background: #f2f2f2;
    text-align: center;
    padding: 40px;
}

.container {
    background: white;
    width: 500px;
    margin: auto;
    padding: 30px;
    border-radius: 10px;
}

textarea {
    width: 100%;
    height: 120px;
    margin: 10px 0;
}

input {
    width: 100%;
    padding: 10px;
    box-sizing: border-box;
}

button {
    padding: 10px 20px;
    margin: 10px;
}

</style>

</head>

<body>

<div class="container">

<h1>Columnar Transposition</h1>

<textarea id="input" 
placeholder="Masukkan teks"></textarea>

<input id="key" 
placeholder="Masukkan kata kunci">

<br>

<button onclick="encrypt()">Enkripsi</button>

<button onclick="decrypt()">Dekripsi</button>

<textarea id="result" 
placeholder="Hasil" readonly></textarea>

</div>


<script>

function getOrder(key) {

    return [...key]
        .map((char, index) => ({
            char: char,
            index: index
        }))
        .sort((a, b) => {

            if (a.char === b.char)
                return a.index - b.index;

            return a.char.localeCompare(b.char);

        })
        .map(item => item.index);
}


function encrypt() {

    let text =
        document.getElementById("input").value;

    let key =
        document.getElementById("key").value;

    if (!key) {
        alert("Masukkan kata kunci!");
        return;
    }

    let columns = key.length;

    let rows =
        Math.ceil(text.length / columns);

    let matrix = [];

    let index = 0;

    for (let r = 0; r < rows; r++) {

        matrix[r] = [];

        for (let c = 0; c < columns; c++) {

            matrix[r][c] =
                text[index++] || "";

        }
    }

    let order = getOrder(key);

    let result = "";

    for (let c of order) {

        for (let r = 0; r < rows; r++) {

            result += matrix[r][c];

        }
    }

    document.getElementById("result").value =
        result;
}


function decrypt() {

    let text =
        document.getElementById("input").value;

    let key =
        document.getElementById("key").value;

    if (!key) {
        alert("Masukkan kata kunci!");
        return;
    }

    let columns = key.length;

    let rows =
        Math.ceil(text.length / columns);

    let matrix =
        Array.from(
            {length: rows},
            () => Array(columns).fill("")
        );

    let order = getOrder(key);

    let index = 0;

    for (let c of order) {

        for (let r = 0; r < rows; r++) {

            if (index < text.length) {

                matrix[r][c] =
                    text[index++];

            }
        }
    }

    let result = "";

    for (let r = 0; r < rows; r++) {

        for (let c = 0; c < columns; c++) {

            result += matrix[r][c];

        }
    }

    document.getElementById("result").value =
        result;
}

</script>

</body>

</html>
