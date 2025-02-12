<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bestellung</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f4f4f4;
        }
        .container {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            width: 300px;
        }
        label, select, input {
            display: block;
            width: 100%;
            margin-bottom: 10px;
        }
        button {
            background: blue;
            color: white;
            padding: 10px;
            border: none;
            cursor: pointer;
            width: 100%;
            border-radius: 5px;
        }
        button:hover {
            background: darkblue;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>Bestellung aufgeben</h2>
    <form id="orderForm">
        <label for="name">Vorname *</label>
        <input type="text" id="name" name="name" required>

        <label for="nachname">Nachname *</label>
        <input type="text" id="nachname" name="nachname" required>

        <label for="handynummer">Handynummer *</label>
        <input type="text" id="handynummer" name="handynummer" required>

        <label for="menge">Menge *</label>
        <input type="number" id="menge" name="menge" min="1" required>

        <label for="verpackung">Wie soll das Brot verpackt werden? *</label>
        <input type="text" id="verpackung" name="verpackung" placeholder="z. B. 3x 50 Brote" required>

        <label for="lieferung">Lieferung gewünscht? *</label>
        <select id="lieferung" name="lieferung" onchange="toggleAdresse()" required>
            <option value="">-- Bitte wählen --</option>
            <option value="Ja">Ja</option>
            <option value="Nein">Nein</option>
        </select>

        <div id="lieferadresse-container" style="display: none;">
            <label for="adresse">Lieferadresse *</label>
            <input type="text" id="adresse" name="adresse">
        </div>

        <label for="datum">Lieferdatum *</label>
        <input type="date" id="datum" name="datum" required>

        <label for="brotTyp">Brot-Typ *</label>
        <select id="brotTyp" name="brotTyp" required>
            <option value="">-- Bitte wählen --</option>
            <option value="Normal">Normal</option>
            <option value="Trocken">Trocken</option>
        </select>

        <button type="submit">Bestellen</button>
    </form>
</div>

<script>
    function toggleAdresse() {
        const lieferung = document.getElementById("lieferung").value;
        const adresseContainer = document.getElementById("lieferadresse-container");
        adresseContainer.style.display = lieferung === "Ja" ? "block" : "none";
        document.getElementById("adresse").required = lieferung === "Ja";
    }

    document.getElementById("orderForm").addEventListener("submit", function(event) {
        if (!this.checkValidity()) {
            alert("Bitte füllen Sie alle Pflichtfelder aus.");
            return;
        }

        event.preventDefault();
        const formData = new FormData(this);

        fetch("https://formspree.io/f/xldgwnea", {
            method: "POST",
            headers: { "Accept": "application/json" },
            body: formData
        })
        .then(response => response.json())
        .then(data => {
            if (data.ok) {
                alert("Bestellung erfolgreich gesendet!");
                this.reset();
                toggleAdresse();
            } else {
                alert("Fehler beim Senden der Bestellung.");
            }
        })
        .catch(error => alert("Fehler: " + error));
    });
</script>

</body>
</html>
