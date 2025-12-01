# Latina-America-
<!doctype html>
<html lang="uk">
<head>
  <meta charset="utf-8" />
  <title>Інтерактивна карта — Архітектура Латинської Америки</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <!-- Leaflet CSS -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
    integrity="sha256-sA+e2X0f3v3wWv+f8FqkQ7l+6GvK9vYg1Pa0jv0zY2M=" crossorigin=""/>
  <style>
    body { margin:0; padding:0; font-family: Arial, sans-serif; }
    #map { position:fixed; top:0; bottom:0; right:0; left:0; }
    .popup-img { display:block; margin:6px 0; max-width:220px; height:auto; }
    .popup-title { font-weight:700; margin-bottom:4px; }
    .popup-caption { font-size: 0.9em; color:#333; }
  </style>
</head>
<body>
  <div id="map"></div>

  <!-- Leaflet JS -->
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"
    integrity="sha256-o9N1j8b0gqGg6i1z6pQqP8s7s6gqGg6i1z6pQqP8s7s=" crossorigin=""></script>

  <script>
    // Ініціалізація карти (центруємо на Латинській Америці)
    const map = L.map('map').setView([-10.0, -60.0], 4);

    // OpenStreetMap шар
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 19,
      attribution: '&copy; OpenStreetMap contributors'
    }).addTo(map);

    // Дані пам'яток: назва, координати, ім'я файлу на Wikimedia (Special:FilePath буде доставляти зображення),
    // короткий підпис/опис.
    const places = [
      {
        id: 'cristo',
        title: "Christ the Redeemer (Cristo Redentor)",
        coords: [-22.95194, -43.21056],
        file: "Christ_the_Redeemer_-_Cristo_Redentor.jpg",
        caption: "Статуя Христа-Спасителя на горі Корковаду, Ріо-де-Жанейро."
      },
      {
        id: 'brasilia_cathedral',
        title: "Cathedral of Brasília (Catedral Metropolitana)",
        coords: [-15.7923, -47.8720],
        file: "Catedral_Metropolitana_de_Bras%C3%ADlia_-_Bras%C3%ADlia_-_20150603150521.jpg",
        caption: "Кафедральний собор у м. Бразиліа, проєкт Оскара Німейєра (модернізм)."
      },
      {
        id: 'mexico_cathedral',
        title: "Mexico City Metropolitan Cathedral",
        coords: [19.434183, -99.133499],
        file: "Mexico_City%2C_Metropolitan_Cathedral_%2820693413381%29.jpg",
        caption: "Кафедральний собор Мехіко — зведений на руїнах ацтекського храму (1573–1813)."
      },
      {
        id: 'las_lajas',
        title: "Sanctuary of Las Lajas (Базиліка Лас-Лахас)",
        coords: [0.811111, -77.585999], // приблизні координати (Ipiales / Las Lajas)
        file: "Santuario_de_Las_Lajas%2C_Ipiales%2C_Colombia%2C_2015-07-21%2C_DD_21-23_HDR.jpg",
        caption: "Неоготична базиліка на мосту над каньйоном річки Гуайтара, Колумбія."
      },
      {
        id: 'santiago_fort',
        title: "Fortaleza San Pedro de la Roca (Castillo del Morro)",
        coords: [20.0177, -75.8036],
        file: "Castillo_de_San_Pedro_de_la_Roca.jpg",
        caption: "Фортеця біля Сантьяго-де-Куба (захист від піратів, 17 ст.)."
      }
    ];

    // Додаємо маркери з попапами (вміст попапа = картинка + підпис)
    places.forEach(place => {
      // використаємо Special:FilePath щоб доставити сам файл (Wikimedia)
      const imgUrl = "https://commons.wikimedia.org/wiki/Special:FilePath/" + place.file;
      const popupHtml =
        `<div class="popup-card">` +
          `<div class="popup-title">${place.title}</div>` +
          `<img class="popup-img" src="${imgUrl}" alt="${place.title}" loading="lazy">` +
          `<div class="popup-caption">${place.caption}</div>` +
          `<div style="margin-top:6px; font-size:0.8em;"><a target="_blank" href="${imgUrl}">Джерело: Wikimedia Commons</a></div>` +
        `</div>`;

      L.marker(place.coords).addTo(map)
        .bindPopup(popupHtml, { maxWidth: 260 });
    });

    // При наведенні курсора: відкривати попап (hover behavior)
    // Leaflet не має вбудованого hover для marker -> додаємо події
    map.eachLayer(function(layer) {
      // нічого — ми додаємо власні обробники нижче
    });

    // Додаємо hover: відкриваємо при mouseover, закриваємо при mouseout
    // (працює на маркери, які ми створили — знайдемо їх через map._layers)
    for (const id in map._layers) {
      const layer = map._layers[id];
      if (layer && layer.getLatLng && layer.getPopup) {
        layer.on('mouseover', function(e) {
          this.openPopup();
        });
        layer.on('mouseout', function(e) {
          this.closePopup();
        });
      }
    }

    // Підказка: якщо хочете змінювати список маркерів — правте масив places.
  </script>
</body>
</html>
