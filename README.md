# Herbarium-Digital
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Herbarium Digital</title>
    <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs"></script>
    <script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/mobilenet"></script>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; }
        .container { width: 80%; margin: auto; }
        .search-box { margin: 20px 0; }
        .plant-list { display: flex; flex-wrap: wrap; justify-content: center; }
        .plant-card { border: 1px solid #ddd; padding: 10px; margin: 10px; width: 200px; }
        .plant-card img { width: 100%; height: auto; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Herbarium Digital</h1>
        <input type="file" id="imageUpload" accept="image/*" onchange="identifyPlant()">
        <div id="result"></div>
        <div class="plant-list" id="plantList">
            <!-- Data tanaman akan dimasukkan di sini -->
        </div>
    </div>

    <script>
        const plants = {
            "rose": { name: "Mawar", image: "https://via.placeholder.com/150", description: "Tanaman berbunga merah." },
            "jasmine": { name: "Melati", image: "https://via.placeholder.com/150", description: "Bunga putih yang harum." },
            "frangipani": { name: "Kamboja", image: "https://via.placeholder.com/150", description: "Bunga khas daerah tropis." }
        };

        async function identifyPlant() {
            const image = document.getElementById("imageUpload").files[0];
            if (!image) return;

            const reader = new FileReader();
            reader.onload = async function(event) {
                const imgElement = document.createElement("img");
                imgElement.src = event.target.result;
                imgElement.width = 200;
                document.getElementById("result").innerHTML = "<p>Memproses...</p>";
                document.getElementById("result").appendChild(imgElement);

                const model = await mobilenet.load();
                const predictions = await model.classify(imgElement);

                document.getElementById("result").innerHTML = `<h3>Hasil Deteksi: ${predictions[0].className}</h3>`;
                displayPlantInfo(predictions[0].className.toLowerCase());
            };
            reader.readAsDataURL(image);
        }

        function displayPlantInfo(plantKey) {
            const plant = plants[plantKey];
            if (plant) {
                document.getElementById("plantList").innerHTML = `
                    <div class="plant-card">
                        <img src="${plant.image}" alt="${plant.name}">
                        <h3>${plant.name}</h3>
                        <p>${plant.description}</p>
                    </div>
                `;
            } else {
                document.getElementById("plantList").innerHTML = "<p>Tanaman tidak ditemukan dalam database.</p>";
            }
        }
    </script>
</body>
</html>
