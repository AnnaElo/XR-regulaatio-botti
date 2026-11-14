# Tietopohja

Botti vastaa vain tämän kansion tiedoista ja niiden ensisijaisista lähteistä.

| Kansio | Sisältö |
| --- | --- |
| `eu/` | EU-säädökset ja -linjaukset, Suomen valvova viranomainen ja kansalliset tulkintaohjeet |
| `fi/` | Suomen kansallinen lainsäädäntö, jolla ei ole omaa EU-säädöstiedostoa |
| `standards/` | Standardit. `availability` kertoo, onko standardi avoin (`open`, `free-download`) vai maksullinen (`paid`) |
| `use-cases/` | Käyttötapaushuomiot: mitä sääntelyä tyypillinen XR-tilanne nostaa esiin (esim. ympäristöä kuvaavat AR-lasit, droonikuvaus, terveydenhuolto) |
| `watchlist/` | Kehitteillä olevat standardit |
| `deadlines.yaml` | Määräpäivät |
| `background-reading.yaml` | Taustalukemisto |
| `_template.yaml` | Pohja uudelle tiedostolle |

Periaatteet:
- Lähteet linkitetään aina ensisijaiseen lähteeseen (EUR-Lex, Finlex, standardin julkaisija).
- EU-säädöksiin lisätään Suomen viranomaisen (esim. Traficom, tietosuojavaltuutettu) tulkintaohjeet, kun ne tarkentavat soveltamista Suomessa.
- Tekstit kirjoitetaan suomeksi ja englanniksi (`fi`, `en`).
- Botti suosittelee ensin avoimesti verkossa saatavia standardeja (`open`, `free-download`) ja mainitsee maksulliset ISO/IEC-, MPEG-I- ja IEEE-standardit toissijaisesti. Maksullisessa standardissa `open_alternative` kertoo saman tarpeen kattavan avoimen standardin.
- Standardin `related_regulation` ja säädöksen `related_standards` kertovat, mihin säädökseen standardi liittyy (esim. EAA → EN 301 549, eIDAS → OpenID4VP).
- `use_cases` (education, culture, industry, healthcare) ja `content_sources` (ai-generated, drone, laser-scanning, phone-camera, 3d-artist) auttavat kohdentamaan suosituksia. Tyhjä lista tarkoittaa kaikkia.
- Kenttä `todo` kertoo, mitä ei ole vielä varmistettu ensisijaisesta lähteestä. Botti kertoo tällöin käyttäjälle, että tieto on tarkistettava.
- `jurisdiction` kertoo, mitä markkina-aluetta tieto koskee (FI, EU, US, global).

Lähtöaineisto: [XR-vuosi 2026](https://annaelo.github.io/XR-vuosi-2026/#standardit), Anna Eloranta (Oamk), FINverse-hanke.
