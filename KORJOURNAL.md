# Körjournal — Allt du behöver veta (för digital implementation i Sverige)

## Vad är en körjournal?

En körjournal är ett löpande register över körningar med ett tjänstefordon (eller privatbil i tjänsten). Den används primärt som skatteunderlag för att styrka hur ett fordon har använts — privat respektive i tjänsten — och är Skatteverkets primära krav för att korrekt beräkna eller reducera bilförmånsvärdet.

Det finns ingen lag som *kräver* att en körjournal förs, men *utan* en körjournal kan den anställde och arbetsgivaren inte styrka en lägre förmånsbeskattning eller reseersättning. Konsekvensen är att Skatteverket beskattar den anställde på ett (ofta högre) schablonmässigt beräknat förmånsvärde.

---

## Juridisk grund och regelverk

| Källa | Vad den styr |
|---|---|
| **Inkomstskattelagen (IL) 61 kap.** | Definierar bilförmån och hur förmånsvärdet beräknas |
| **Skatteverkets allmänna råd SKV A 2022:1** (och föregångare) | Konkretiserar vad körjournalen ska innehålla och hur den används |
| **Socialavgiftslagen** | Arbetsgivaravgifter beräknas på förmånsvärdet — körjournalen påverkar underlaget |
| **Inkomstskattelagen 12 kap. 5 §** | Reglerar avdragsrätt för resor med egen bil i tjänsten |
| **Bokföringslagen** | Körjournalen kan utgöra verifikation i bokföringen (t.ex. milersättning) |

---

## De fyra skattemässiga syftena

### 1. Styrka obetydlig privat körning (≤10 resor och ≤100 mil/år)
Om den anställde kan visa att bilen körts privat **högst 10 gånger** och **högst 100 mil** under ett år anses bilen inte utgöra en skattepliktig bilförmån. Körjournalen är det enda sättet att styrka detta.

### 2. Reducera förmånsvärdet med 25 % (nedsättning vid tjänstekörning ≥3 000 mil)
Om bilen körts **minst 3 000 mil i tjänsten** under ett år får förmånsvärdet sättas ned med 25 %. Körjournalen är bevis för att milgränsen uppnåtts.

### 3. Fördela drivmedelsförmån
Om arbetsgivaren betalar drivmedel används körjournalen för att separera tjänstekörningens drivmedel (inte skattepliktigt för den anställde) från privatkörnningens drivmedel (skattepliktigt). Utan körjournal beskattas allt drivmedel som förmån.

### 4. Trängselskatt
Om arbetsgivaren betalar trängselskatt och bilen passerar betalstationerna både privat och i tjänsten används körjournalen för att avgöra vilka passager som är skattepliktiga förmåner.

---

## Obligatoriska och rekommenderade fält

Skatteverket har inte ett formellt lagkrav på exakt vilka fält som ska finnas, men anger i vägledning och allmänna råd att körjournalen ska innehålla tillräcklig information för att kontrollera körningens karaktär.

### Obligatoriska uppgifter (i praktiken)
| Fält | Notering |
|---|---|
| Registreringsnummer | Identifierar fordonet |
| Namn på förare | |
| Datum | Per resa/körning |
| Mätarställning start | OBD-värde eller manuell avläsning |
| Mätarställning slut | |
| Körda kilometer | Slut minus start (eller GPS-beräknat) |
| Resväg (från → till) | Adressnivå — "kontoret" räcker inte ensamt |
| Ärende/Uppdrag | Syftet med resan; t.ex. "kundbesök Volvo Göteborg" |
| Privat/Tjänst | Klassificering av körningen |

### Rekommenderade tilläggsfält
| Fält | Varför det är värdefullt |
|---|---|
| Besökta företag / kontaktpersoner | Stärker tjänstekaraktären vid en granskning |
| Tankningar (liter + kostnad) | Underlag för drivmedelsförmånsbeskattning |
| Ingående mätarställning (periodens start) | Möjliggör avstämning mot Transportstyrelsen |
| Utgående mätarställning (periodens slut) | Överförs till nästa period |
| Attest / underskrift | Bekräftar att uppgifterna är korrekta |

### Från Visma Spiris mall (den bifogade PDF:en)
Mallen organiserar körjournalen per **månad** med:
- Rubrikrad: År/månad, Period, Reg.nr, Namn, Anställningsnummer, Avdelning
- Per körning: Datum, Ärende/Uppdrag, Resväg (från–till), Mätarställning start/slut, Körda km
- Summering: Totalt antal körda km, varav Privat körning
- Ingående och utgående mätarställning (manuellt överförda mellan perioder)
- Attest med datum och underskrift

---

## Digital körjournal — vad som gäller

Skatteverket godkänner elektroniska körjournaler. Kravet är att den **innehåller samma uppgifter** som en manuell journal. Det finns inget krav på att använda ett specifikt system eller format.

### Vad ett digitalt system måste klara
1. **Alla obligatoriska fält** måste finnas och vara ifyllda per resa
2. **Körningar får inte kunna raderas eller ändras i efterhand** utan att det loggas — Skatteverket ser positivt på system med auditlogg
3. **Export/utskrift** måste kunna genereras för valfri period (månadsvis är standard)
4. **Attestering** — det ska gå att bekräfta (signera) att journalen är korrekt; digitalt godkännande fungerar
5. **Mätarställning** — bör synkas mot OBD/ECU-värde (inte enbart GPS-baserat) för att matcha fordonets faktiska odometer som Transportstyrelsen har registrerat

### GPS-baserade system — begränsningar
- **GNSS-avstånd ≠ OBD-odometer**: GPS räknar den faktiska spårade sträckan (kortare vid tunnlar/dålig signal); OBD-odometern räknar hjulvarv. Dessa skiljer sig typiskt 1–3 %. Vid Skatteverkets granskning är **OBD-odometern den relevanta referenspunkten**.
- **Tunnlar och parkeringsgarage** skapar GPS-luckor — systemet bör hantera detta transparent (t.ex. "position ej tillgänglig 2 min, 1,2 km interpolerat").
- **Adressupplösning** (reverse geocoding) från GPS-koordinater ger inte alltid exakta gatuadresser — kontrollera kvaliteten, särskilt i industriområden och på landsbygd.
- **Privat vs. tjänst** — ett GPS-system kan inte automatiskt klassificera körningens *ärende*. Klassificeringen måste göras av föraren (eller en administratör). Automatisk klassificering baserad på geofences (t.ex. hemadress = privat) är ett hjälpmedel men inte tillräckligt för Skatteverket utan förarens bekräftelse.

### Format och lagring
- Ingen specifik filformat är föreskriven — PDF, Excel, CSV godkänns alla
- **Lagringskrav:** Skatteverket kan granska upp till **6 år** tillbaka (IL 49 kap., taxeringslagen). Data måste vara läsbar och exporterbar under hela perioden.
- **GDPR:** Körjournaldata är personuppgifter — se avsnittet om GDPR nedan

---

## Hur körjournalen används i praktiken

### Arbetsflöde (manuell/digital)
```
Körning sker
  → Föraren registrerar: datum, från, till, ärende, privat/tjänst
  → Mätarställning noteras (start + slut)
  → Månadsvis: föraren summerar och attesterar
  → Arbetsgivaren/löneadm. attesterar
  → Underlag lämnas till lönehantering för eventuell förmånsjustering
  → Arkiveras (minst 6 år)
```

### Koppling till lön och förmånsbeskattning
- Lönesystemet beräknar bilförmånsvärdet per månad
- Om tjänstekörning understiger 3 000 mil/år tillämpas inget nedsättningsavdrag
- Drivmedelsförmån beräknas månadsvis på antal privata mil × Skatteverkets schablonvärde per mil
- Körjournalen är **verifikationsunderlaget** för dessa beräkningar — utan den kan revisorn inte styrka siffrorna

### Milersättning (privatbil i tjänsten)
Om anställda kör sin **privata bil** i tjänsten och vill ha skattefri milersättning (18,50 kr/mil skattefritt 2024) krävs körjournal som visar:
- Datum
- Ärende
- Från → till (med adresser)
- Antal körda mil

Ersättning upp till schablonbeloppet är skattefri; överskjutande belopp är skattepliktigt.

---

## GDPR-aspekter (specifikt för Sverige)

Körjournaldata innehåller personuppgifter (namn, rörelsemönster, besökta platser). IMY (Integritetsskyddsmyndigheten) har följande relevans:

| Aspekt | Vad det innebär |
|---|---|
| **Rättslig grund** | Behandling av körjournaldata är nödvändig för att uppfylla en **rättslig förpliktelse** (skattelagstiftning) — Art. 6.1.c GDPR. Inget samtycke krävs. |
| **Ändamålsbegränsning** | Data insamlad för körjournal får inte användas för t.ex. prestandaövervakning utan ytterligare rättslig grund |
| **Lagringstid** | Skatteverkets 6-åriga granskningsmöjlighet motiverar 6 års lagring — definiera och dokumentera detta |
| **Transparens** | Anställda ska informeras om att körjournalen förs, vad som registreras och hur länge |
| **Åtkomst** | Anställd har rätt att begära ut sina egna körjournaluppgifter |
| **Säkerhet** | Körjournaldata ska skyddas — inte vara åtkomlig för obehöriga chefer eller kollegor |

---

## Vanliga fallgropar och Skatteverkets granskningspunkter

| Problem | Risk |
|---|---|
| Körningar registrerade i efterhand (t.ex. hela månaden på en gång) | Skatteverket ifrågasätter trovärdigheten; kan leda till att hela körjournalen underkänns |
| "Kontoret" som resväg utan gatuadress | Otillräcklig specificering — komplettera med fullständig adress |
| Ärendefältet tomt eller "diverse" | Kan inte styrka tjänstekaraktär |
| GPS-avstånd stämmer inte med OBD-mätaren | Förklaringsproblem vid granskning |
| Körningar saknas (t.ex. från helger) | Skatteverket kan anse att privata körningar dolts |
| Ingen attest | Svagt bevisvärde |
| Systemet tillåter retroaktiva ändringar utan logg | Kan underkännas helt |

---

## Sammanfattning: Minimikrav för en godkänd digital körjournal

En digital körjournal godkänns av Skatteverket om den:

- [ ] Registrerar varje körning med datum, från, till, ärende och km
- [ ] Inkluderar mätarställning (start och slut) per körning **eller** per period med OBD-synkad odometer
- [ ] Låter föraren klassificera privat / tjänst per körning
- [ ] Är attesterad av föraren (och gärna av arbetsgivaren) månadsvis
- [ ] Kan exporteras per månad och år i läsbart format
- [ ] Sparas i minst 6 år
- [ ] Inte tillåter ostörd retroaktiv redigering (auditlogg rekommenderas)
- [ ] Hanterar personuppgifter enligt GDPR med dokumenterad rättslig grund och lagringstid
