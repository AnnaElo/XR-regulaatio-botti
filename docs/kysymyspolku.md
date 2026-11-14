# Kysymyspolku ja päättely

Botti selvittää ensin käyttäjän tilanteen ja vertaa sitä tietopohjan `applies_when`-ehtoihin. Taulukko näyttää, mitkä vastaukset nostavat mitkäkin lähteet esiin. Botin varsinainen ohje on tiedostossa [`prompts/system-prompt.md`](../prompts/system-prompt.md).

## Kysymykset

| # | Kysymys | Miksi kysytään |
| --- | --- | --- |
| 1 | Markkina-alue: Suomi, Eurooppa vai maailmanlaajuinen (myös Yhdysvallat)? | Rajaa sääntelyn `jurisdiction`-kentän mukaan. USA:ta ei vielä kateta. |
| 2 | Mitä rakennetaan: sovellus, laite, alusta tai virtuaalimaailma, web-kokemus, 3D- tai paikkatietosisältö? | Valitsee standardiryhmät ja tuotesäädökset (CRA, Data Act). |
| 3 | Kerätäänkö katse-, kasvo-, ääni-, liike- tai sijaintidataa tai muita henkilötietoja? Onko käyttäjissä lapsia? | GDPR, art. 9 ja vaikutustenarviointi. |
| 4 | Käytetäänkö tekoälyä: hahmot, chatbotit, generoitu sisältö, tunteentunnistus, ihmisten arviointi? | AI Actin velvoitteet ja riskiluokka. |
| 5 | Toimiala ja käyttöympäristö: koulutus, kulttuuri, teollisuus vai terveydenhuolto; kuluttajat, työntekijät, opiskelijat, potilaat vai julkiset palvelut? | Tunteentunnistuksen kielto ja suuririskisyys, MDR, saavutettavuus, toimialan standardit. |
| 6 | Mistä 3D-sisältö tulee: tekoäly, drooni, laserkeilain, kännykkäkamera vai 3D-artisti? Kuvaako laite ympäristöä käytön aikana? | Drooni- ja tietosuojasäännöt, tekoälysisällön merkintä, sisällön standardit. |
| 7 | Alustan ominaisuudet: käyttäjien sisältö, kirjautuminen tai digitaalinen identiteetti, maksut tai tokenit, verkkoon liitetyt laitteet? | DSA, eIDAS, MiCA, Data Act. |
| 8 | Organisaation koko ja vaihe: konsepti, prototyyppi vai markkinoilla? | Mikroyritysten vapautukset (EAA), NIS2:n kokorajat, aikataulut. |

Botti kysyy enintään kolme asiaa kerrallaan eikä kysy uudelleen sitä, minkä käyttäjä on jo kertonut.

## Mitä vastaukset nostavat esiin

| Vastaus | Säädökset | Standardit |
| --- | --- | --- |
| Henkilötietoja, myös testikäyttäjiltä | `gdpr` | |
| Katse-, kasvo- tai liikedata käyttäjän tunnistamiseen | `gdpr` (art. 9, 35) | |
| Tekoälyhahmo, chatbot tai generoitu sisältö | `aiact` (art. 50) | |
| Tunteentunnistus | `aiact` (art. 5, 50) | |
| Tekoäly arvioi ihmisiä koulutuksessa tai työssä | `aiact` (liite III) | |
| Käyttäjien tuottama tai jakama sisältö | `dsa` | |
| Verkkoon liitetty laite tai datapalvelu | `dataact` | |
| Digitaalinen tuote myydään EU:ssa | `cra` | |
| Kirjautuminen tai sähköinen tunnistaminen | `eidas` | `oidc`, `did`, `vc` |
| Kryptovarat tai tokenit | `mica` | |
| Kuluttajapalvelu, esim. verkkokaupan AR-sovitus | `eaa` | `design` |
| Organisaatio kuuluu NIS2-toimialoihin | `nis2` | |
| Natiivi XR-sovellus useille laitteille | | `openxr` |
| Selaimessa toimiva XR | | `webxr`, `design` |
| 3D-sisällön siirto ja jakelu | | `gltf`, `openusd` |
| Paikkatieto, kaupunkimallit, digitaaliset kaksoset | | `ogcfeat`, `ogctiles`, `3dtiles`, `citygml`, `geopose` |
| AR-lasit tai puhelin kuvaa, skannaa tai kuuntelee ympäristöä (myös reaaliaikainen käännös) | `use-cases/ar-capture`, `gdpr`, `aiact` | `webxr`, `openxr` |
| Kirjautuminen ja katseenseuranta | `use-cases/vr-login-eyetracking`, `gdpr`, `aiact` | `oidc`, `openxr` |
| Droonikuvaus, laserkeilaus, fotogrammetria | `use-cases/reality-capture`, `uas`, `gdpr` | `pointcloud`, `cog`, `3dtiles`; maksullinen `astm-e57` |
| Tekoälyllä tuotettu 3D-sisältö | `use-cases/ai-generated-3d`, `aiact` | `c2pa`, `gltf` |
| Koulutus | `use-cases/education-xr`, `aiact`, `digipalvelulaki` | `xaur`, `vc`; maksulliset `ieee-1589`, `iso-iec-5927` |
| Kulttuuri ja museot | `use-cases/culture-xr`, `digipalvelulaki` | `gltf`, `3dtiles`, `xaur`, `c2pa` |
| Teollisuus | `use-cases/industry-xr`, `aiact`, `cra`, `dataact` | `openusd`, `ifc`; maksullinen `ieee-1589` |
| Terveydenhuolto | `use-cases/healthcare-xr`, `mdr`, `ehds`, `gdpr` | `dicom`, `fhir`; maksulliset `medical-device-standards`, `ieee-3079` |
| Julkisen sektorin tai EAA:n piiriin kuuluva palvelu | `eaa`, `digipalvelulaki` | `en301549`, `wcag`, `xaur` |

Maksulliset standardit (`availability: paid`, esim. `mpeg-i`, `iso-9241`, `iso-iec-mar`) mainitaan aina avointen jälkeen omana kohtanaan.

Linjaukset `web40` ja `ep2024` ovat taustatietoa: botti mainitsee ne vain, jos käyttäjä kysyy EU:n strategiasta.

## Vastauksen rakenne

1. Yhteenveto
2. Todennäköisesti sovellettava sääntely: vaatimus, perustelu (mikä käyttäjän kertoma täsmäsi), artikla, voimaantulo, Suomen viranomainen, lähdelinkki
3. Suositellut avoimet standardit
4. Muut huomioitavat (maksulliset) standardit
5. Avoimet kysymykset
6. Huomautus: tekoälyn tuottama yleiskatsaus, ei oikeudellista neuvontaa

Testitapaukset vaiheen 4 testaukseen ovat tiedostossa [`tests/cases.yaml`](../tests/cases.yaml).
