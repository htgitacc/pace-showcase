# Kiadásnapló — egy kis, AI-s alkalmazás végigvitele a PACE kereten, a felületről indítva

> Generált fájl. Forrás: `90-ops/case-study.json`. Ne szerkeszd kézzel.
> Státusz: **publikálva** (2026-10-30T11:48:38Z)

A kiadas egy helyben futó kiadásnapló egyszemélyes háztartásnak, kategória-javaslattal: kulccsal a DeepSeek, kulcs nélkül egy kulcsszó-szabály javasol. Egy AI-ügynök a PACE keretben, a keret konzoljáról indítva vitte végig a brieftől a működő POC-ig és a G4-es próbáig, emberi jóváhagyású kapukkal; a cél a keret második végigvitele idegen projekten, a felületről. 2026-10-30-án mérve: a G0–G5 mindegyike jóváhagyással zárult (a G4 és a G5 fenntartással), egyik sem lépte túl a két bírálati kört; a próbán mind a négy hipotézis igazolódott, kettő kimondott mérési korláttal. A kör DeepSeek-költsége Tibor egyenleg-leolvasása szerint 0,01 USD a 0,10 USD-s keretből; az alkalmazás saját összesítője 0,008650 USD-t számolt.

## A probléma

A probléma a brief szerint: a kiadásokat a felhasználó nem jegyzi fel azonnal, ezért a hónap végén nincs megbízható, kategóriánkénti adata. A cél: a hónap végén egy pillantással látni a kategóriánkénti költést, és a tételeket gyorsan, pontosan felvenni. Öt siker-kritérium (gyors felvétel, pontos havi összesítő, használható kategória-javaslat, olcsó kulcsos validáció, rövid tesztelés) és négy hipotézis (H1–H4) mérhető alakban állt a briefben. A projekt saját fejlesztésű tesztprojekt: a keret (PACE) képességét méri, nem piacra készül.

## A megközelítés

- Indítás a keret konzoljáról, a brief AI-átnézésével (DeepSeek, csúcsidő-őrrel); minden javaslatról emberi döntés született.
- Minden fázis kapuval zárult: gépi lánc, független bíráló írási jog nélkül, legfeljebb két kör, majd emberi döntés; a nem blokkoló megállapítás ticketet kapott.
- A kódot a builder ügynök építette hét mérföldkőben, Node.js beépített moduljaival, futásidejű függőség nélkül: előbb a teszt, mutációs méréssel és fej nélküli Edge-ben futó felületi mérőszkripttel.
- A mérés hihetőségéért: a kulcsszó-táblázatot egy hideg futás írta a mintalista ismerete nélkül (a git-történetben igazolható sorrenddel), és a mintalista mellé egy valódi bemeneten futó kontroll került.
- Az alkalmazás saját élő DeepSeek-hívásai csak kedvezményes időben és naplózva indultak (kivétel a keret 2026-10-25-i élő próbájának két, naplózatlan hívása — lásd a korlátokat); a tesztek hamis végpontot használnak, kitalált kulccsal.

## Bizonyíték számokkal

- **100** — A kapuk aránya, amelyek jóváhagyással zárultak (G0–G5) (2026-10-30-án, a publikáláskor (KIADAS-C-010) a forrásból mérve); a köztes átdolgozást a napló egy fájlba vonja, ezért ez végső kimenet, nem „elsőre átment” arány — a kiadasban rework döntés nem született. _(mérőszám: `gate_approval_rate`, forrás: ../pace-kiadas/70-measure/flow-metrics.json)_
- **1.3** — Bírálati körök átlaga kapunként (2026-10-30-án, a publikáláskor (KIADAS-C-010) a forrásból mérve): a G0, a G1, a G4 és a G5 egy kör, a G2 és a G3 két kör; egyik kapu sem lépte túl a két körös korlátot. _(mérőszám: `gate_rework_rounds`, forrás: ../pace-kiadas/70-measure/flow-metrics.json)_
- **100** — A forrásolt állítások aránya a persona és a kockázati regiszter claim-jein (2026-10-30-án, a publikáláskor (KIADAS-C-010) a forrásból mérve): mindegyik saját bevallású (self_reported) vagy valódi evidence-re hivatkozik; a kiadasban a claim-ek a briefből épültek, kutatás nélkül. _(mérőszám: `sourced_claim_coverage_rate`, forrás: ../pace-kiadas/70-measure/flow-metrics.json)_
- **100** — SC1 / H1: a G4-es próba 10 tételéből 10 került fel 30 mp alatt (medián 11,2 mp, a leghosszabb 21,3 mp), 2026-10-27-én mérve. _(forrás: ../pace-kiadas/80-build/trial-2026-10-27.md (npm run entry-times -- data/trial.sqlite); ../pace-kiadas/gates/G4.md)_
- **15** — H3: a kulcsszó-szabály a 20 tételes mintalistán 15/20-at ért el (küszöb 12/20), a próba tíz valódi tételén 6/10-et; a mintalista leírásai átlagosan 2.65, a próbatételek 1.10 szavasak — a 15/20 a valódi beírásra valószínűleg optimista. _(forrás: ../pace-kiadas/90-ops/retro-log.json (KIADAS-RETRO-001); KIADAS-C-009 verified_by)_
- **0.01** — SC4: Tibor leolvasása a DeepSeek-fiókon 2026-10-30-án 0,01 USD fogyást mutatott a 0,10 USD-s keretből; az alkalmazás összesítője 0,008650 USD-t számolt (ebből 0,005 USD a keret két naplózatlan hívásának felső becslése), az eltérés +0,001350 USD. A leolvasás az alkalmazás költségnaplójában is rögzítve, és a kerete alatt marad. _(forrás: ../pace-kiadas/data/costs.sqlite (balance_readings, GET /api/costs); ../pace-kiadas/20-backlog/backlog.json (KIADAS-E-003, megválaszolt kérdés))_
- **0** — Nyitott kérdések (2026-10-30-án, a publikáláskor (KIADAS-C-010) a forrásból mérve): a kapukon és utánuk feltett kérdések mind megválaszolva, a publikálásról szólót is beleértve (KIADAS-C-010). _(mérőszám: `open_questions_count`, forrás: ../pace-kiadas/70-measure/flow-metrics.json)_
- **2.8** — Emberi döntések kapunként (a felhasználó által megválaszolt kérdések a lezárt kapukon) (2026-10-30-án, a publikáláskor (KIADAS-C-010) a forrásból mérve). _(mérőszám: `human_decisions_per_gate`, forrás: ../pace-kiadas/70-measure/flow-metrics.json)_

## Tanulságok

- A kiértékelési mintát a valódi beírási stílushoz kell igazítani, és mellé valódi bemenetű kontroll kell: a hosszú, körülíró mintaleírásokon mért eredmény a rövid valódi beírásra optimista. A tanulság a keret tervezési módszerkártyájába került (PACE-C-203). _(KIADAS-RETRO-001)_
- Egy mérőszkript külső függőségét (itt a magától frissülő böngészőt) a mérés feltevéseként kell kezelni, és a hibaágát is mérni kell: a frissítés után a felületi mérés a G4 bírálatán nem futott, és folyamatokat hagyott hátra; a G5 előtt javítva és befecskendezett hibával mérve (KIADAS-B-001). _(KIADAS-RETRO-001)_
- Az építés közbeni feltevések nyitott kérdésként gyűltek, és a kapun egyben dőltek el; az egyik builder-megfigyelésből ticket lett, és a G5 előtt javítva (KIADAS-B-002: üres leírásra a kulcsos javaslat nem hív DeepSeeket). _(KIADAS-RETRO-001)_
- A tévedés a saját szövegünkben is továbbélhet: a G4 bírálati jelentésének egy pontatlan mondatát („a négy kontroll-tévesztés mind egyszavas”) az ügynök ellenőrzés nélkül vitte át a retróba és a keret módszerkártyájába; a G5 bírálója fogta meg, és mindkét helyen a tévedés kimondásával javítva (KIADAS-C-011, PACE-C-205). _(KIADAS-RETRO-001)_

## Korlátok — amit ez a case study NEM bizonyít

- Egyfelhasználós próba: a G4-es próbát egy ember tartotta, 10 tétellel, egy estén; a hipotézisek egy hónapnyi valódi használaton nem mértek.
- A H2 (DeepSeek 20/20) és a H3 (kulcsszó-szabály 15/20) egy könnyű, hosszú leírásokból álló mintalistán mért; a valódi, rövid beírásra a kontroll csak tíz tételt ad (KIADAS-RETRO-001).
- Az SC4 összevetése durva felbontású: Tibor leolvasása két tizedes pontosságú érték (0,01 USD), a fiók kerekítési módját nem mértük; a leolvasás azt igazolja, hogy a fogyás a keret alatt maradt, a +0,001350 USD-s eltérés viszont a leolvasás felbontásán belül van, ezért nem mérhető vele, hogy a felső becslés pontos-e. A keret 2026-10-25-i élő próbájának két hívása nincs naplózva (felső becslés 0,005 USD; az okának ticketje: PACE-C-206).
- Az SC5 mért 10 perce a próba lépéslistájának 7. lépése nélküli szám, mert az ügynök a lépés eredményét a próba előtt nem írta ki (KIADAS-RETRO-001, PACE-C-204).

