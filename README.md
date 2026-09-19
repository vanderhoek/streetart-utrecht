# Street Art Utrecht — De Nood-Familiedag 2026

De street art wandeling voor **De Nood-Familiedag 2026**, zondag 20 september.
Start op de **Wittevrouwensingel 71**, eindigt bij **En Public** aan het Griftpark,
met halverwege koffie bij **Geogap** aan de Nieuwegracht.

11 stops, ±5,7 km, ruim een uur lopen. Daarnaast 20 extra's elders in de stad,
waaronder een zijlus door de Watervogelbuurt (+1,8 km) langs de KBTR op een gans.

Alles zit in één bestand: `index.html`. Geen build, geen npm, geen framework.
Dubbelklikken is genoeg.

## Wat zit erin

- Interactieve kaart (Leaflet + OpenStreetMap) met genummerde markers en de route als stippellijn
- Kaart en lijst praten met elkaar: klik een kaart-marker → de beschrijving scrollt in beeld, klik een stop → de kaart vliegt ernaartoe
- Foto's van Wikimedia Commons, met fotograaf en licentie onder elke foto
- Een uitklapbare lijst met street art elders in de stad (Ondiep, Pijlsweerd, Overvecht, Kanaleneiland, Lombok)
- Donker/licht thema, en een layout die op de telefoon onderweg gewoon werkt

## De route aanpassen

Bovenin het `<script>`-blok in `index.html` staan twee arrays: `ROUTE` en `EXTRAS`.
Een stop ziet er zo uit:

```js
{
  title: "De KBTR op een gans",
  artist: "onbekend",              // optioneel
  year: 2019,                      // optioneel
  place: "Gansstraat",
  lat: 52.078576, lng: 5.129035,
  text: "Beschrijving, mag HTML bevatten.",
  approx: true,                    // optioneel: toont 'locatie bij benadering'
  tags: ["straatpoëzie"],          // optioneel
  list: ["meerdere werken", "op één plek"],  // optioneel
  photo: {                         // optioneel
    src: "https://...", by: "Fotograaf", lic: "CC BY 2.0", page: "https://commons.wikimedia.org/..."
  }
}
```

De volgorde van `ROUTE` bepaalt de nummering, de lijn op de kaart én de afstand —
die wordt bij het laden uitgerekend, dus je hoeft de kilometers nergens bij te werken.
`kind: "terminus"` maakt van een stop een groen start- of eindpunt.

### Eigen foto's gebruiken

Zet je foto's in een map `photos/` naast `index.html` en verwijs er relatief naar:

```js
photo: { src: "photos/kbtr.jpg", by: "Luuk van der Hoek", lic: "eigen foto", page: "" }
```

## Lokaal draaien

`index.html` openen in je browser volstaat. Wil je 'm serveren:

```bash
python -m http.server 8000
# → http://localhost:8000
```

## Online zetten

De repo is publiek en GitHub Pages staat aan: **Settings → Pages → Source: `main` / root**.
De site staat op <https://vanderhoek.github.io/streetart-utrecht/>.

## Bronnen en credits

- Kaart: [OpenStreetMap](https://www.openstreetmap.org/copyright)-bijdragers, via [Leaflet](https://leafletjs.com)
- Foto's: Wikimedia Commons — fotograaf en licentie staan bij elke foto in de app
- Routegegevens: de [Street Art wandeling van Wij Wandelen](https://www.wij-wandelen.nl/street-art-wandeling-in-utrecht/),
  de [Street Art route van Ontdek Utrecht](https://www.ontdek-utrecht.nl/route/67841/street-art-route)
  en [Discover Utrecht](https://www.discover-utrecht.com/collection/street-art/)
- Coördinaten: [Nominatim](https://nominatim.openstreetmap.org)

De murals zijn van hun makers; de kunstenaar staat waar bekend bij elke stop vermeld.
Straatkunst verandert — een mural kan intussen overgeschilderd zijn.
