# Street Art Utrecht — De Nood-Familiedag 2026

De street art wandeling voor **De Nood-Familiedag 2026**, zondag 20 september.
Start op de **Wittevrouwensingel 71**, eindigt bij **En Public** aan het Griftpark,
met tegen het eind een rondleiding bij **Geogap** aan de Nieuwegracht.
De route loopt tegen de klok in: eerst westwaarts langs Vogelenbuurt en Pijlsweerd,
dan zuid de binnenstad in, en via de Nieuwegracht weer omhoog.

13 stops, 6,37 km, ruim vijf kwartier lopen (looproute, door Valhalla uitgerekend). Daarnaast 21 extra's elders in de stad,
waaronder een zijlus door de Watervogelbuurt (+1,8 km) langs de KBTR op een gans.

Alles zit in één bestand: `index.html`. Geen build, geen npm, geen framework — wel even
lokaal serveren in plaats van dubbelklikken, zie *Lokaal draaien*.

## Wat zit erin

- Interactieve kaart (Leaflet + OpenStreetMap) met genummerde markers en de **echte looproute**,
  straat voor straat — vooraf uitgerekend met Valhalla en in de pagina gebakken, dus geen routeserver nodig
- Per stop een uitklapbare achtergrondtekst van 100–200 woorden
- Kaart en lijst praten met elkaar: klik een kaart-marker → de beschrijving scrollt in beeld, klik een stop → de kaart vliegt ernaartoe
- Foto's van Wikimedia Commons, met fotograaf en licentie onder elke foto
- Een uitklapbare lijst met street art elders in de stad (Ondiep, Pijlsweerd, Overvecht, Kanaleneiland, Lombok)
- **Je eigen positie op de kaart**: de browser vraagt bij het openen om locatie en zet een blauwe
  stip met nauwkeurigheidscirkel neer, die meeloopt. Het richtkruis rechtsboven op de kaart zet
  ’m aan/uit en vliegt naar je toe. Werkt alleen op https — dus op de gepubliceerde site, niet vanaf schijf
- Donker/licht thema, en een layout die op de telefoon onderweg gewoon werkt (getest op 320–430 px)

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
  more: `<p>100-200 woorden achtergrond, uitklapbaar in de kaart.</p>`,
  photo: {                         // optioneel; mag ook een array van meerdere foto's zijn
    src: "https://...", by: "Fotograaf", lic: "CC BY 2.0", alt: "...",
    ratio: "16/9",                 // optioneel, standaard 4/3
    span: true,                    // optioneel: volle breedte in een raster van meerdere foto's
    page: "https://commons.wikimedia.org/..."
  }
}
```

De volgorde van `ROUTE` bepaalt de nummering. `kind: "terminus"` maakt van een stop een groen
start- of eindpunt.

⚠️ Let op: de gelopen route staat los van de stoplijst. Verander je de volgorde of voeg je een stop toe,
dan klopt `ROUTE_PATH_ENC` (de ingebakken straatgeometrie) en `ROUTE_KM` / `ROUTE_MIN` niet meer.
Die haal je opnieuw op bij Valhalla, profiel `pedestrian`:

```
https://valhalla1.openstreetmap.de/route?json={"locations":[{"lat":..,"lon":..},...],"costing":"pedestrian"}
```

De `shape` per leg is een polyline met precisie 6; decodeer die en zet 'm om naar de delta-codering
die `decodePath()` in de pagina verwacht (lat/lon om en om, in stappen van 1e-5 graad).

### Eigen foto's gebruiken

Zet je foto's in een map `photos/` naast `index.html` en verwijs er relatief naar:

```js
photo: { src: "photos/kbtr.jpg", by: "Luuk van der Hoek", lic: "eigen foto", page: "" }
```

## Lokaal draaien

⚠️ **Niet dubbelklikken.** `tile.openstreetmap.org` weigert verzoeken zonder herkomst, dus vanaf een
`file://`-pagina krijg je 403's in plaats van kaarttegels. Serveer 'm lokaal:

```bash
python -m http.server 8000
# → http://localhost:8000
```

## Online zetten

De repo is publiek. Zet Pages aan via **Settings → Pages → Source: Deploy from a branch → `main` / `/ (root)`**.

Omdat er een custom domain (`vanderhoek.io`) op de user-site-repo `vanderhoek.github.io` staat, erven
alle project pages dat domein. De site komt dus op <https://vanderhoek.io/streetart-utrecht/>,
niet op `vanderhoek.github.io/...` — die URL stuurt door.

## Bronnen en credits

- Kaart: [OpenStreetMap](https://www.openstreetmap.org/copyright)-bijdragers, via [Leaflet](https://leafletjs.com)
- Looproute: [Valhalla](https://valhalla1.openstreetmap.de), de routeservice van OpenStreetMap
- Achtergrond bij de muurformules: [Utrechtse muurformules](https://muurformules.sites.uu.nl/), Universiteit Utrecht
- Foto's: Wikimedia Commons — fotograaf en licentie staan bij elke foto in de app
- Routegegevens: de [Street Art wandeling van Wij Wandelen](https://www.wij-wandelen.nl/street-art-wandeling-in-utrecht/),
  de [Street Art route van Ontdek Utrecht](https://www.ontdek-utrecht.nl/route/67841/street-art-route)
  en [Discover Utrecht](https://www.discover-utrecht.com/collection/street-art/)
- Coördinaten: [Nominatim](https://nominatim.openstreetmap.org)

De murals zijn van hun makers; de kunstenaar staat waar bekend bij elke stop vermeld.
Straatkunst verandert — een mural kan intussen overgeschilderd zijn.
