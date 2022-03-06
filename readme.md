# Vefforritun 2, 2021, hópverkefni 1

[Kynning í fyrirlestri](https://youtu.be/W0k01_KRE4I).

Útbúa skal vefþjónustur fyrir veitingastað sem bíður upp á fyrir óauðkenndan notanda að:

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
  * Flokkur, krafist, vísun í flokka töflu
  * Dagsetningu sem vöru var bætt við, útbúið sjálfkrafa
  * Dagsetningu sem vara var uppfærð, útbúið sjálfkrafa

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

* `/menu`
  * `GET` Skilar síðu af vörum á matseðli raðað í dagsetningar röð, nýjustu vörur fyrst
  * `POST` býr til nýja vöru á matseðil ef hún er gild og notandi hefur rétt til að búa til vöru, aðeins ef notandi sem framkvæmir er stjórnandi
  * Bæði er í lagi að taka við gögnum sem `form data` þar sem bæði mynd og gögn eru send inn, eða sem `JSON` og útfæra annað route sem tekur við mynd og festir við vöru, t.d. `POST /menu/{id}/image`
* `/menu?category={category}`
  * `GET` Skilar síðu af vörum í flokk, raðað í dagsetningar röð, nýjustu vörur fyrst
* `/menu?search={query}`
  * `GET` Skilar síðu af vörum þar sem `{query}` er í titli eða lýsingu, raðað í dagsetningar röð, nýjustu vörur fyrst
  * Það er hægt að senda bæði `search` og `category` í einu
* `/menu/:id`
  * `GET` sækir vöru
  * `PATCH` uppfærir vöru, aðeins ef notandi sem framkvæmir er stjórnandi
  * `DELETE` eyðir vöru, aðeins ef notandi sem framkvæmir er stjórnandi
* `/categories`
  * `GET` skilar síðu af flokkum
  * `POST` býr til flokk ef gildur og skilar, aðeins ef notandi sem framkvæmir er stjórnandi
* `/categories/:id`
  * `PATCH` uppfærir flokk, aðeins ef notandi sem framkvæmir er stjórnandi
  * `DELETE` eyðir flokk, aðeins ef notandi sem framkvæmir er stjórnandi

### Karfa, vefþjónustur

* `/cart`
 * `POST` býr til körfu og skilar
* `/cart/:cartid`
  * `GET` skilar körfu með `id` jafnt `:cartid` með öllum línum og reiknuðu heildarverði körfu
  * `POST` bætir vöru við í körfu, krefst fjölda og auðkennis á vöru
  * `DELETE` eyðir körfu með `id` jafnt `:cartid`, þarf að kalla í til að eyða körfu eftir að pöntun varð til
* `/cart/:cartid/line/:id`
  * `GET` skilar línu í körfu með `id` jafnt `:cartid` með fjölda og upplýsingum um vöru
  * `PATCH` uppfærir fjölda í línu, aðeins fyrir línu í körfu með `id` jafnt `:cartid`
  * `DELETE` eyðir línu úr körfu, aðeins fyrir línu í körfu með `id` jafnt `:cartid`

### Pöntun, vefþjónustur

* `/orders`
  * `GET` skilar síðu af pöntunum, nýjustu pantanir fyrst, aðeins ef notandi er stjórnandi
  * `POST` býr til pöntun með viðeigandi gildum, skilar stöðu á pöntun og auðkenni
* `/orders/:id`
  * `GET` skilar pöntun með öllum línum, gildum pöntunar, stöðu pöntunar og reiknuðu heildarverði körfu
* `/orders/:id/status`
  * `GET` skilar pöntun með stöðu pöntunar og lista af öllum stöðubreytingum hennar
  * `POST` uppfærir stöðu pöntunar, aðeins ef notandi er stjórnandi (var upprunalega `PATCH` en `POST` á frekar við)

### Notendur, vefþjónustur

* `/users/`
  * `GET` skilar síðu af notendum, aðeins ef notandi sem framkvæmir er stjórnandi
* `/users/:id`
  * `GET` skilar notanda, aðeins ef notandi sem framkvæmir er stjórnandi
  * `PATCH` breytir hvort notandi sé stjórnandi eða ekki, aðeins ef notandi sem framkvæmir er stjórnandi og er ekki að breyta sér sjálfum
* `/users/register`
  * `POST` staðfestir og býr til notanda. Skilar auðkenni og netfangi. Notandi sem búinn er til skal aldrei vera stjórnandi
* `/users/login`
  * `POST` með netfangi (eða notandanafni) og lykilorði skilar token ef gögn rétt
* `/users/me`
  * `GET` skilar upplýsingum um notanda sem á token, auðkenni og netfangi, aðeins ef notandi innskráður
  * `PATCH` uppfærir netfang, lykilorð eða bæði ef gögn rétt, aðeins ef notandi innskráður

Aldrei skal skila eða sýna hash fyrir lykilorð.

## WebSockets (WS)

Meðan notandi á pöntun sem ekki er í `FINISHED` stöðu getur notandi test WebSocket þjón og byrjað á því að senda inn auðkenni pöntunar. Við það fær notandi uppfærslur á stöðu pöntunar um leið og þær gerast.

Stjórnendur/starfsfólk veitingastaðs geta einnig tengst WebSocket þjón og sent inn JWT auðkenni. Með því fær það uppfærslur um allar pantanir sem koma inn og stöðubreytingar á þeim. Athugið að _ekki_ er farið sérstaklega yfir þetta í námsefni.

Ekki er gerð krafa um að sambandi milli client og server sé viðhaldið og endurvakið ef eitthvað kemur upp.

## Myndir

Allar myndir skal geyma í [Cloudinary](https://cloudinary.com/) eða [imgix](https://imgix.com/), bæði þær sem settar eru upp í byrjun og þær sem sendar eru inn gegnum vefþjónustu.

Aðeins ætti að leyfa myndir af eftirfarandi tegundum (`mime type`):

* jpg, `image/jpeg`
* png, `image/png`

## Notendaumsjón

Notendaumsjón skiptist í tvennt: óauðkenndur notandi og stjórnendur.

* Óauðkenndur notandi getur skoðað matseðil og sett í körfu.
* Óauðkenndur notandi getur sett mat af matseðli í körfu og fengið til baka auðkenni fyrir pöntun.
* Óauðkenndur notandi getur fylgst með stöðu pöntunar gegnum vefþjónustu og WS
* Stjórnendur geta breytt, bætt við, og eytt efni á matseðli

Þar sem óauðkenndur notandi útbýr pöntun skal það teljast nægjanlegt að sá notandi eigi auðkenni körfu/pöntunar til að mega sýsla með hana. Ekki er tiltekið _hvernig_ notandi eigi að geyma þessi gögn, aðeins að það sé gert eftir að karfa er búin til.

Nota skal JWT með passport og geyma notendur i gagnagrunni. Útfæra þarf auðkenningu, nýskráningu notanda og middleware sem passar upp á heimildir stjórnenda.

Útbúa skal í byrjun einn stjórnanda með notandanafn `admin` og þekkt lykilorð, skrá skal lykilorð í `README` verkefnis.

## Tæki, tól og test

Setja skal upp eslint fyrir JavaScript. Engar villur skulu koma fram ef npm run lint er keyrt. Leyfilegt er að skilgreina hvaða reglusett er notað, ekki er krafa um að nota það sem hefur verið notað í öðrum verkefnum.

Setja skal upp jest til að skrifa test. Skrifa skal test fyrir a.m.k.:

* fjóra endapunkta, þar sem
* a.m.k. einn krefst auðkenningar
* a.m.k. einn tekur við gögnum

Í `README` skal tiltaka hvernig test eru keyrð.

## Annað

Allar niðurstöður sem geta skilað mörgum færslum (fleiri en 10) skulu skila _síðum_.

Ekki þarf að útfæra „týnt lykilorð“ virkni.

Setja skal upp vefinn á Heroku tengt við GitHub með Heroku postgres settu upp.

## Sýnilausn

Sýnilausn er keyrandi á:
`https://vef2-2022-h1-synilausn.herokuapp.com/`

Til að testa ws tengingu:

* Logga sig inn sem admin og fá token
  * `POST https://vef2-2022-h1-synilausn.herokuapp.com/users/login`
  * `{ "username": "admin", "password": "1234567890" }`
* Logga sig inn á WS fyrir admin
  * `wss://vef2-2022-h1-synilausn.herokuapp.com/admin`
  * Header `Authorization: Bearer <token>`
  * Ath þarf að refactora þ.a. token sé sent í byrjun því [WS staðall styður ekki headers almennt](https://devcenter.heroku.com/articles/websocket-security#authentication-authorization)
* Búa til körfu
  * `POST https://vef2-2022-h1-synilausn.herokuapp.com/cart`
* Bæta í körfu
  * `POST https://vef2-2022-h1-synilausn.herokuapp.com/cart/<CART UID>`
  * `{ "product": 5, "quantity": 1 }`
* Búa til pöntun úr körfu
  * `POST https://vef2-2022-h1-synilausn.herokuapp.com/orders`
  * `{ "cart": "<CART UID>", "name": "test" }`
* Sjá pöntun verða til í WS tengingu fyrir admin
* Tengjast WS fyrir client
  * `wss://vef2-2022-h1-synilausn.herokuapp.com/orders/<ORDER UID>`
* Breyta stöðu á pöntun sem admin
  * `POST https://vef2-2022-h1-synilausn.herokuapp.com/orders/<ORDER UID>`
  * `{ "status": "PREPARE" }`
* Sjá nýja stöðu koma inn á pöntunar WS

## Hópavinna

Verkefnið skal unnið í hóp, helst með þremur einstaklingum. Hópar með tveim eða fjórum einstaklingum eru einnig í lagi, ekki er dregið úr kröfum fyrir færri í hóp en gerðar eru auknar kröfur ef fleiri en þrír einstaklingar eru í hóp.

Hægt er að auglýsa eftir hóp á slack á rásinni #vef2-2022-vantar-hóp.

Hafið samband við kennara ef ekki tekst eða ekki er mögulegt að vinna í hóp.

## README

Í rót verkefnis skal vera `README.md` skjal sem tilgreinir:

* Upplýsingar um hvernig setja skuli upp verkefnið
* Dæmi um köll í vefþjónustu m.v. test gögn
* Innskráning fyrir `admin` stjórnanda ásamt lykilorði
* Nöfn og notendanöfn allra í hóp

## Mat

* 20% Tæki, tól og test. `README` uppsett, verkefni keyrir á Heroku. Test gögn uppsett.
* 20% Auðkenning og notendaumsjón, uppsetning og vefþjónustur
* 20% Matseðils vefþjónustur, myndir studdar
* 30% Karfa og pantanir vefþjónustur
* 10% WebSockets virkni

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
  * `osk`

---

> Útgáfa 0.4

| Útgáfa | Breyting                                                                                         |
|--------|--------------------------------------------------------------------------------------------------|
| 0.1    | Fyrsta útgáfa                                                                                    |
| 0.2    | Skilgreining á vefþjónustum, mat, um tæki og tól, ekki krafa um tengitöflu fyrir flokka og vörur |
| 0.3    | Bæta við `POST` `/cart` og `POST` `/orders/:id/status`                                           |
| 0.4    | Bæta við um sýnilausn                                                                            |
