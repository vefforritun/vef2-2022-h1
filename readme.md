# Vefforritun 2, 2021, hópverkefni 1

Útbúa skal vefþjónustur fyrir veitingastað sem bíður upp á fyrir óauðkenndan notanda:

* skoða matseðil
* leið til að setja vörur af matseðli „í körfu“
* búa til pöntun

Fyrir starfsmenn veitingastaðs/eldhús sem eru innskráðir er hægt að:

* sjá opnar pantanir
* breyta stöðum á pöntunum
* vinna með matseðil

## Töflur

Til að útfæra þessa virkni þarf að útbúa gagnagrunn með eftirfarandi töflur, lágmarksvirkni er skilgreind, ef þið viljið bæta við er það frjálst.

### Matseðill, töflur

* Flokkur
  * Auðkenni, einstakt
  * Titill, einstakur, ekki tómur
* Vörur
  * Auðkenni, einstakt
  * Titill, einstakur, ekki tómur
  * Verð, heiltala, krafist
  * Lýsing, lengri texti, krafist
  * Mynd, krafist, url á mynd
  * Dagsetningu sem vöru var bætt við, útbúið sjálfkrafa
  * Dagsetningu sem vara var uppfærð, útbúið sjálfkrafa
* Flokkur–vörur tenging, vara getur verið í fleiri en einum flokk
  * Auðkenni flokks
  * Auðkenni vöru

### Karfa, töflur

* Karfa
  * Auðkenni, einstakt, **ekki** skal nota `serial` gildi heldur langan streng (t.d. `uuid`)
  * Dagsetning búið til, útbúið sjálfkrafa
* Línur í körfu
  * Auðkenni vöru
  * Auðkenni körfu
  * Fjöldi vara, heiltala stærri en 0

### Pöntun, töflur

* Pöntun
  * Auðkenni, einstakt, **ekki** skal nota `serial` gildi heldur langan streng (t.d. `uuid`)
  * Dagsetning búið til, útbúið sjálfkrafa
  * Nafn, strengur, krafist
* Línur í pöntun
  * Auðkenni vöru
  * Auðkenni körfu
  * Fjöldi vara, heiltala stærri en 0
* Stöður pantana
  * Auðkenni pöntunar
  * Staða pöntunar
  * Dagsetning farið í stöðu, útbúið sjálfkrafa

Stöður pöntunar eru:

* `NEW`, pöntun er komin inn en ekkert hefur verið gert
* `PREPARE`, pöntun er móttekin af starfsmönnum/eldhúsi og er í undirbúningi
* `COOKING`, verið er að elda það sem er í pöntun
* `READY`, pöntun er tilbúin til afhendingar til viðskiptvinar
* `FINISHED`, pöntun hefur verið afhend viðskiptavin

Pöntun getur og mun eiga sér allar stöður.

Ekki þarf að útfæra nema eina leið í gegnum stöður, þ.e.a.s. ekki er hægt að fara úr seinni stöðu í fyrri eða hoppa á milli staða. Stöður fara áfram í nákvæmlega þeirri röð sem þær eru skráðar hér.

Þar sem töflur fyrir körfu, pöntun og línur eru mjög líkar er leyfilegt að nýta sama töflustrúktúr.

## Gögn

Ekki eru gefin gögn til að vinna með. Þið ákveðið og útfærið eigin gögn sem sett eru inn í byrjun. Þetta geta verið plat gögn (t.d. með því að nota [faker](https://github.com/faker-js/faker)) eða gögn frá ykkar uppáhalds veitingstað.

Í byjun verða að vera a.m.k. fimm mismunandi flokkar (með a.m.k. þrem vörum í) og a.m.k. 15 vörur.

Útbúa skal a.m.k. tvær pantanir með mismunandi stöður í kerfinu.

## Vefþjónustur

Útfæra skal vefþjónustur til að útfæra alla virkni. Nota skal `JSON` í öllum samskiptum.

Staðfesta þarf og hreinsa öll gögn sem send eru inn af notendum, bæði óauðkenndum og stjórnendum.

GET á `/` skal skila lista af slóðum í mögulegar aðgerðir.

Þegar eining er búin til skal það aðeins gert ef notandi er stjórnandi og eining er rétt.

Ekki þarf að útfæra meira en tilgreint er.

Ef beðið er um eitthvað sem ekki er til skal skila `404`.

Ef beðið er um einingu eða reynt að framkvæma aðgerð sem ekki er leyfi fyrir skal skila `401`.

### Matseðill, vefþjónustur

TBD

### Karfa, vefþjónustur

TBD

### Pöntun, vefþjónustur

TBD

## WebSockets (WS)

Meðan notandi á pöntun sem ekki er í `FINISHED` stöðu getur notandi test WebSocket þjón og byrjað á því að senda inn auðkenni pöntunar.

Við það fær notandi uppfærslur á stöðu pöntunar um leið og þær gerast.

Stjórnendur/starfsfólk veitingastaðs geta einnig tengst WebSocket þjón og sent inn JWT auðkenni. Með því fær það uppfærslur um allar pantanir sem koma inn og stöðubreytingar á þeim.

## Myndir

Gefnar eru myndir fyrir sjónvarpsþætti í `img/`.

Allar myndir skal geyma í [Cloudinary](https://cloudinary.com/) eða [imgix](https://imgix.com/), bæði þær sem settar eru upp í byrjun og þær sem sendar eru inn gegnum vefþjónustu.

Aðeins ætti að leyfa myndir af eftirfarandi tegundum (`mime type`):

* jpg, `image/jpeg`
* png, `image/png`

## Notendaumsjón

Notendaumsjón skiptist í tvennt: óauðkenndur notandi og stjórnendur.

* Óauðkenndur notandi getur skoðað matseðil og sett í körfu.
* Óauðkenndur notandi getur sett mat af matseðli í körfu og fengið til baka auðkenni fyrir pöntun.
* Óauðkenndur notandi getur fylgst með stöðu pöntunar gegnum vefþjónustu og WS
* Notendur geta skráð einkunn fyrir sjónvarpsþátt, heiltölugildi frá og með 0 til og með 5
* Stjórnendur geta breytt, bætt við, og eytt efni á matseðli

Þar sem óauðkenndur notandi útbýr pöntun skal það teljast nægjanlegt að sá notandi eigi auðkenni körfu/pöntunar til að mega sýsla með hana. Ekki er tiltekið _hvernig_ notandi eigi að geyma þessi gögn, aðeins að það sé gert eftir að karfa er búin til.

Nota skal JWT með passport og geyma notendur i gagnagrunni. Útfæra þarf auðkenningu, nýskráningu notanda og middleware sem passar upp á heimildir stjórnenda.

Útbúa skal í byrjun einn stjórnanda með notandanafn `admin` og þekkt lykilorð, skrá skal lykilorð í `README` verkefnis.

## Annað

Allar niðurstöður sem geta skilað mörgum færslum (fleiri en 10) skulu skila _síðum_.

Ekki þarf að útfæra „týnt lykilorð“ virkni.

Setja skal upp vefinn á Heroku tengt við GitHub með Heroku postgres settu upp.

## Sýnilausn

Í lok febrúar verður gefin út keyrandi sýnilausn sem hægt er að nota til að fá betri tilfinningu fyrir kröfum og útfærslu.

## Hópavinna

Verkefnið skal unnið í hóp, helst með þremur einstaklingum. Hópar með tveim eða fjórum einstaklingum eru einnig í lagi, ekki er dregið úr kröfum fyrir færri í hóp en gerðar eru auknar kröfur ef fleiri en þrír einstaklingar eru í hóp.

Hægt er að auglýsa eftir hóp á slack á rásinni #vef2-2022-hópur.

Hafið samband við kennara ef ekki tekst eða ekki er mögulegt að vinna í hóp.

## README

Í rót verkefnis skal vera `README.md` skjal sem tilgreinir:

* Upplýsingar um hvernig setja skuli upp verkefnið
* Dæmi um köll í vefþjónustu
* Innskráning fyrir `admin` stjórnanda ásamt lykilorði
* Nöfn og notendanöfn allra í hóp

## Mat

TBD

## Sett fyrir

Verkefni sett fyrir í fyrirlestri miðvikudaginn 9. febrúar 2022.

## Skil

Tilnefna skal hópstjóra sem skráir sig í ákveðinn hóp undir „Hópverkefni 1“ í Canvas. Aðrir nemendur skrá sig í framhaldinu í sama hóp.

Hópstjóri skal skila fyrir hönd allra og skila skal í Canvas í seinasta lagi föstudaginn 18. mars.

Skil skulu innihalda:

* GitHub notendanöfn allra (passa þarf að allir nemendur séu í hópnum!)
* Slóð á verkefni keyrandi á Heroku
* Slóð á GitHub repo fyrir verkefni. Dæmatímakennurum skal hafa verið boðið í repo. Notendanöfn þeirra eru:
  * `MarzukIngi`
  * `WhackingCheese`

---

> Útgáfa 0.1

| Útgáfa | Breyting                     |
|--------|------------------------------|
| 0.1    | Fyrsta útgáfa                |
