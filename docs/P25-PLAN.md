# P25 — Plan de implementare

**Status:** plan aprobat; implementarea aplicației nu a început.

**Scop:** aplicație web pentru reconcilierea extraselor bancare ale unei asociații, cu import din formate diferite, asocierea tranzacțiilor cu membrii și obligațiile, calculul penalităților, rapoarte PDF și portal pentru membri.

Planul pornește de la cerințele P25 furnizate și aprobate de utilizator. Necunoscutele despre reguli și formate reale rămân deschise în [registrul întrebărilor](agents/OPEN-QUESTIONS.md). Constrângerile aprobate sunt în [registrul deciziilor](agents/DECISIONS.md), iar arhitectura țintă este rezumată în [ARCHITECTURE.md](agents/ARCHITECTURE.md).

## Stack

- Next.js și TypeScript
- PostgreSQL și Prisma ORM
- Tailwind CSS
- Autentificare și RBAC
- Vitest pentru teste unitare și de integrare
- Playwright pentru teste end-to-end
- Docker Compose
- GitHub Actions
- Generare PDF pentru rapoarte

Comenzile proiectului și versiunile concrete nu sunt încă verificate; vezi Q-002.

## Arhitectură

Aplicația va avea interfață Next.js, servicii executate pe server și PostgreSQL accesat prin Prisma. Verificarea identității, rolului și mediului de date are loc pe server. Importul și regulile financiare sunt servicii separate de prezentare; fiecare format bancar are propriul parser.

Datele DEMO și REAL sunt strict separate. Serverul determină mediul activ din contextul autorizat, nu dintr-o valoare de încredere trimisă de client. Toate citirile și scrierile sunt limitate la acel mediu, iar referințele între medii sunt respinse. Datele DEMO sunt fictive și marcate vizibil `DEMO`.

## Model de date conceptual

- `User` și `Role` — identitate, autentificare și permisiuni.
- `Apartment` și `Member` — apartamente și membrii asociației.
- `BankAccount`, `Statement` și `Transaction` — conturi, extrase importate și tranzacții.
- `Obligation` — sume datorate pentru perioade; sumele sunt numere întregi de bani RON.
- `Allocation` — partea dintr-o tranzacție atribuită uneia sau mai multor obligații.
- `MatchingRule` — reguli configurabile pentru sugestiile de potrivire.
- `PenaltyRule` — reguli configurabile pentru penalități.
- `AuditEvent` — jurnalul acțiunilor relevante.
- Mediu (`DEMO`/`REAL`) — domeniul fiecărei înregistrări financiare și al relațiilor sale.

Schema Prisma detaliată și constrângerile relaționale se definesc în etapa 2. Ordinea alocării plăților, perioada de grație și parametrii penalităților reale rămân blocați de întrebările deschise.

## API propus

Rutele de mai jos sunt grupări de lucru, nu contracte API aprobate. Contractele exacte se definesc înainte de implementarea fiecărui flux.

- `/api/imports` — încărcare, previzualizare, validare și confirmare idempotentă a importului.
- `/api/reconciliation` — tranzacții, sugestii explicabile și decizii manuale.
- `/api/allocations` — alocarea și revizuirea plăților.
- `/api/penalties` — calcul pe baza regulilor configurate.
- `/api/reports` — datele și generarea PDF pentru lista de plată și raportul de reconciliere.
- `/api/member` — datele accesibile membrului autentificat.

Toate operațiile cu date verifică autorizarea și mediul pe server.

## Fluxuri utilizator

1. Administratorul lucrează într-un mediu clar marcat DEMO sau REAL.
2. Încarcă un extras; parserul corespunzător produce o previzualizare cu erori și duplicate.
3. După confirmare, tranzacțiile valide se importă atomic și idempotent.
4. Motorul propune potriviri după referință, nume, IBAN cunoscut și sumă, cu scor și explicație.
5. Cazurile incerte ajung la revizuire manuală.
6. Administratorul confirmă potrivirile și alocă plăți parțiale, avansuri, supraplăți și plăți pentru mai multe luni, conform regulilor aprobate.
7. Sistemul calculează penalitățile din reguli configurate și generează rapoarte PDF.
8. Membrul autentificat își poate accesa numai informațiile proprii.

## Testare

- **Vitest, unitar:** parsere și validări, chei de deduplicare, sume în bani întregi, scoruri și explicații de potrivire, alocări, penalități și ecuația soldului.
- **Vitest, integrare:** import tranzacțional și idempotent, relații și constrângeri Prisma, separarea DEMO/REAL, verificări de rol și acces la date.
- **Playwright, E2E:** previzualizare și confirmare import, revizuire manuală, generarea rapoartelor și portalul membrului cu acces limitat.
- Testele de business trebuie să acopere și cazurile-limită ale fiecărei reguli importante.

Raportul de reconciliere trebuie să închidă exact: **sold inițial + încasări − cheltuieli = sold final**.

## Riscuri și controale

- **Amestecarea datelor DEMO/REAL:** mediul este stabilit server-side; citirile, scrierile și relațiile sunt limitate la același mediu.
- **Acces la datele altui membru:** verificarea rolului și a apartenenței se face pe server pentru fiecare operație.
- **Import repetat sau parțial:** idempotency și tranzacții de bază de date, cu previzualizare înainte de confirmare.
- **Potrivire incorectă:** explicații păstrate, prag configurat după clarificare și revizuire manuală pentru cazurile incerte.
- **Reguli reale necunoscute:** statutul, scadențele, penalitățile și ordinea alocării nu se inventează; rămân Q-005–Q-008.
- **Format real necunoscut:** parser-ele reale așteaptă răspunsul Q-004; formatele demo nu se prezintă drept extrase autentice.

## Etape și criterii de acceptare

1. **Fundament:** repository configurat cu Next.js, TypeScript, PostgreSQL, Prisma, Docker și structură de directoare; aplicația și baza pornesc prin comenzile documentate.
2. **Model de date:** entitățile și relațiile aprobate sunt modelate, migrările se aplică pe o bază goală, iar mediile sunt izolate.
3. **Date demonstrative:** 20 de apartamente, 3 luni de obligații, două formate bancare demo, RON și marcaj DEMO vizibil.
4. **Import bancar:** parser separat pentru fiecare format, previzualizare, validare, deduplicare și import idempotent tranzacțional.
5. **Reconciliere:** potriviri explicabile după criteriile aprobate, revizuire manuală pentru tranzacțiile incerte și reguli învățate cu audit.
6. **Alocări și penalități:** plăți parțiale, avansuri, supraplăți și plăți pe mai multe luni; penalități configurabile și testate.
7. **Rapoarte:** lista de plată PDF și raport de reconciliere; soldul se închide exact.
8. **Portal și securitate:** portal membru, RBAC, autorizare pe server, audit și acces numai la datele permise.
9. **Testare și livrare:** teste Vitest și Playwright, GitHub Actions, Docker și documentație. Deploy-ul necesită o aprobare separată.

## Întrebări deschise

- Q-004 — banca și formatele reale.
- Q-005 — regulile aplicabile din statut.
- Q-006 — termenul de plată și perioada de grație.
- Q-007 — procentul și plafonul penalităților.
- Q-008 — ordinea alocării plăților parțiale.
- Q-009 — pragul potrivirii automate.
- Q-010 — formatul real al listei de plată.

Aceste întrebări nu au răspunsuri presupuse. Dacă demo-ul are nevoie de valori ilustrative, acestea vor fi documentate explicit ca temporare și fictive.
