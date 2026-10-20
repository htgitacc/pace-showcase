# Smart Voice Assistant — egy példaprojekt végigvitele a PACE kereten, brieftől a POC-ig

> Generált fájl. Forrás: `90-ops/case-study.json`. Ne szerkeszd kézzel.
> Státusz: **publikálva** (2026-10-20T14:30:38Z)

A voice egy szimulált pilot: egy hang- és szövegjegyzetelő webalkalmazás (PWA) egy munkahelyi tesztcsoportnak, amelyet egy AI-ügynök a PACE keretben vitt végig a brieftől a működő POC-ig, emberi jóváhagyású kapukkal. A cél nem a termék, hanem annak megmutatása, hogy a keret egy idegen projektet is követhetően és felületen át vezet végig — és közben a keret saját hibáit is feltárja. 2026-10-20-án mérve: a G0–G5 kapu mindegyike jóváhagyással zárult (a G2–G5 fenntartásokkal); a G5 első futását a bíráló egy elavult darabszám miatt visszaküldte, az új futás egy kör után jóváhagyást kapott, és futásonként egyik kapu sem lépte túl a két bírálati kört. A keret maga fejlesztés alatt áll.

## A probléma

A voice tét nélküli teszt- és portfólióprojekt (Tibor döntése): soha nem lesz éles, fizetős API-kulcs és Azure-hozzáférés nincs, a pilot szimulált környezetben fut (laptop és egy Androidos telefon, valódi tesztelők és adatvédelmi szakértő nélkül). A kód a projekt saját repójában él, a keret a sajátjában; a keret szabályai átjönnek, a projekt eltérései a projektben állnak. Az átírás helyben futó nyílt Whisper-modellel (whisper.cpp, turbó) készül; a hangtesztekre Tibor döntése szerint egy másik projektben már igazolt eredmény a mérvadó.

## A megközelítés

- A keret alapvonal-kapui (G0 brief → G1 discovery → G2 scope → G3 építésre kész → G4 POC kész → G5 tanulság) mindegyike determinisztikus lánccal (séma, nyomonkövethetőség, söprés, tesztek, render, szinkron-terv), írási jog nélküli bírálóval és emberi döntéssel zárult; a kapu sosem lépett át magától.
- Az építést mérföldkövenként egy builder ügynök végezte — az M6 builderének briefje kimondottan kérte a teszt-előbbet, és a saját jelentése szerint így dolgozott (az M5 buildere a saját jelentése szerint a kódot a teszt előtt írta; a repó egyiket sem igazolja, mert a kód és a teszt egy commitban érkezett) —, és minden mérföldkő után a keret oldaláról visszamérés jött: teljes teszt, saját mutánsok a kulcs-ágakon, a túlélő mutánsok kimérése.
- Minden menet közbeni döntés fájlba került (ticket, döntési napló, projekt-leírás), az eredeti állítás megmaradt, a meghaladás kimondva áll mellette; a mérhetetlen dolgok név szerint felsorolva, egyik sem állítva teljesültnek.
- A kérdések a konzol felületén mentek Tiborhoz, mindegyik ugyanazzal a szerkezettel: mi a kérdés és mire vonatkozik, mit javaslunk, mi az ára, és mi a másik opció az árával.

## Bizonyíték számokkal

- **100** — A forrásolt (saját bevallású vagy valódi bizonyítékra hivatkozó) állítások aránya a persona és a kockázati regiszter claim-jein. _(mérőszám: `sourced_claim_coverage_rate`, forrás: ../pace-voice/70-measure/flow-metrics.json)_
- **100** — A kapuk aránya, amelyek jóváhagyással zárultak (2026-10-20-án, a publikáláskor (VOICE-C-046) a forrásból mérve; a köztes átdolgozást a napló egy fájlba vonja, ezért ez nem „elsőre átment” arány — a G5 első futása rework döntéssel zárult). _(mérőszám: `gate_approval_rate`, forrás: ../pace-voice/70-measure/flow-metrics.json)_
- **1.5** — Bírálati körök átlaga kapunként, 2026-10-20-án, a publikáláskor (VOICE-C-046) a forrásból mérve. Kapunként számolva: a G5 két futása együtt három kör (az első futás két kör után rework, az új futás egy kör után jóváhagyás); futásonként egyik kapu sem lépte túl a két körös korlátot. _(mérőszám: `gate_rework_rounds`, forrás: ../pace-voice/70-measure/flow-metrics.json)_
- **50** — A kapuk aránya, ahol a bíráló legalább egy SZERZŐDÉSES jelölésű ([SÚLYOS]/[JELZÉS]) megállapítást tett, 2026-10-20-án, a publikáláskor (VOICE-C-046) a forrásból mérve. ALSÓ KORLÁT: a G0–G2 bírálója „(súlyos …)”, „(jelzés …)” alakban, a G4-é részben „[FENNTARTÁS · …]”, „[JAVASLAT · …]” alakban jelölt, ezeket a számláló nem látja (PACE-B-127, PACE-B-129), pedig minden kapun volt megállapítás. _(mérőszám: `critic_finding_rate`, forrás: ../pace-voice/70-measure/flow-metrics.json)_
- **0** — Nyitott kérdések, 2026-10-20-án, a publikáláskor (VOICE-C-046) a forrásból mérve: a kapukon és utánuk feltett kérdések mind megválaszolva, a publikálásról szólókat is beleértve (VOICE-C-046). _(mérőszám: `open_questions_count`, forrás: ../pace-voice/70-measure/flow-metrics.json)_
- **3.5** — Emberi döntések kapunként (a felhasználó által megválaszolt kérdések a lezárt kapukon), 2026-10-20-án, a publikáláskor (VOICE-C-046) a forrásból mérve. _(mérőszám: `human_decisions_per_gate`, forrás: ../pace-voice/70-measure/flow-metrics.json)_
- **23** — A voice végigvitele a keret sok hibáját hozta elő: a pace backlogban 2026-10-03 (a voice indulása) óta 23 olyan bug áll, amelynek leírása a voice-ot említi (2026-10-20-án, a publikáláskor (VOICE-C-046) a forrásból mérve); ebből 18 lezárva, 5 nyitott (PACE-B-127, PACE-B-128, PACE-B-129, PACE-B-130, PACE-B-132). Köztük a voice G1-én és G2-én (PACE-B-086, PACE-B-087), a külső projekt bekötésén (PACE-B-109…PACE-B-114, PACE-B-116), az M1-en (PACE-B-122), a VOICE-SP-002-n (PACE-B-123), az M6-on (PACE-B-124), a G5 előkészítésén (PACE-B-126), a G5-ön, illetve a jóváhagyása után (PACE-B-127…PACE-B-131) és a showcase készítésekor (PACE-B-132) talált hibák. _(forrás: projects/pace/20-backlog/backlog.json (a 2026-10-03 utáni, a leírásukban a voice-ot említő bug típusú elemek))_

## Tanulságok

- Egy idegen projekt a keret hibáit is megmutatja: a voice végigvitele közben a keret sok hibája jött elő — a felvételtől és a külső repó bekötésétől a commit-hookon és az azonosító-foglaláson át a mérésig —, mind ticketként; a nagy többségük teszttel lezárva, a G5 legutóbbi találatai nyitott ticketként várnak. _(VOICE-RETRO-001)_
- Az ügynök által épített kódot a keret oldaláról is vissza kell mérni: a builder túlélő mutánsainak egy része valódi rést mutatott (a lap bekötését csak a böngészős mérés védte), más része kimérve tényleg ekvivalens volt — a levezetés helyett a mérés döntött. _(VOICE-RETRO-001)_
- A mérés is hazudhat, és a javítása is lehet félkész: a bírálati megállapítások aránya csendben alulmért, mert a bíráló a szerződéses jelölés helyett saját alakot írt; az első javítás (PACE-B-126) csak az egyik alakot fedte, és az ügynök ezt teljesnek vette — a G5 bírálója fogta meg. A tanulság a módszerkönyvtárba került (PACE-C-192: a bírálói brief szó szerint kéri a jelölést), az ok-elemzés: VOICE-RCA-001. _(VOICE-RETRO-001, VOICE-RCA-001)_
- A döntés helye a fájl, nem a beszélgetés: a menet közbeni emberi döntések (hangtesztek lezárása, laptopos mérés elégsége, a belépés vágása) ticketként éltek tovább, így a kapu bírálója ellenőrizni tudta, hogy az átfogalmazások őszinték és nem csendes meghaladások. _(VOICE-RETRO-001)_
- A felület akkor működik, ha a kérdés önmagában érthető: a konzol kérdés-paneljén a javaslat, az ár és a másik opció együtt tette lehetővé, hogy Tibor egyetlen ülésben minden kérdést megválaszoljon. _(VOICE-RETRO-001)_

## Korlátok — amit ez a case study NEM bizonyít

- Szimulált környezet: a pilot hipotézisei (H1–H5) valódi tesztcsoport és hónapokig tartó használat nélkül nem igazolhatók; a mérés kódja szimulált adaton ellenőrzött.
- A telefonos viselkedés nagy része nem mért (a telepítés útja, a háttér- és lezárt képernyős felvétel, a böngésző tároló-törlése); a hangra Tibor telefonos próbája és egy másik projektben igazolt eredmény áll — a lista: ../pace-voice/80-build/g4-nem-mert.md.
- Az átírás sebessége nem mérőszám: a helyi modell egy gyenge laptopon fut (Tibor döntése: „nem mért adat”).
- A céges Entra-belépés és a kétfaktoros azonosítás Azure-hozzáférés nélkül nem próbálható; a POC engedélylistás fejlesztői belépést használ, csak helyi elérésen.
- A lap néhány ragasztó-sorát csak a böngészős mérés védi, a láncban futó tesztkészlet nem.

