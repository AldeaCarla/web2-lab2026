# Decisions — web2-lab2026

<!-- graffino-agentic:begin v1 -->
> Constraints the code must obey. Status: `approved`, `proposed`, or `superseded-by <ID>`.
> Changing an approved decision requires the user's approval.
<!-- graffino-agentic:end -->

## Index

- D-001 · Stack-ul aplicației
- D-002 · Precizia sumelor monetare
- D-003 · Importuri idempotente și parsere separate
- D-004 · Potriviri explicabile și revizuire manuală
- D-005 · Penalități configurabile
- D-006 · Separare strictă DEMO/REAL
- D-007 · Autorizare pe server
- D-008 · Închiderea exactă a raportului de reconciliere

---

## D-001 · Stack-ul aplicației
- **Status:** approved
- **Rule:** Aplicația folosește Next.js și TypeScript, PostgreSQL și Prisma, Tailwind CSS, autentificare cu RBAC, Vitest, Playwright, Docker Compose, GitHub Actions și generare PDF pentru rapoarte.
- **Why:** Acesta este stack-ul aprobat pentru proiectul P25.
- **Evidence:** Planul P25 furnizat și aprobat de utilizator.

## D-002 · Precizia sumelor monetare
- **Status:** approved
- **Rule:** Sumele RON se stochează și se calculează ca numere întregi de bani; calculele monetare nu folosesc `float`.
- **Why:** Calculele exacte previn erorile de rotunjire în alocări, penalități și rapoarte.
- **Evidence:** Regula obligatorie din planul P25 aprobat.

## D-003 · Importuri idempotente și parsere separate
- **Status:** approved
- **Rule:** Importul extraselor este idempotent, iar parserul fiecărui format bancar este izolat. Importul confirmat validează și deduplică datele înainte de salvarea tranzacțională.
- **Why:** Reimportul nu trebuie să creeze tranzacții duplicate sau să lase un import parțial.
- **Evidence:** Regulile obligatorii și etapa 4 din planul P25 aprobat.

## D-004 · Potriviri explicabile și revizuire manuală
- **Status:** approved
- **Rule:** Fiecare potrivire automată păstrează explicația și scorul; tranzacțiile incerte ajung la revizuire manuală.
- **Why:** Administratorul trebuie să poată verifica motivul unei potriviri și să decidă cazurile incerte.
- **Evidence:** Regulile obligatorii și etapa 5 din planul P25 aprobat.

## D-005 · Penalități configurabile
- **Status:** approved
- **Rule:** Penalitățile se calculează pe baza regulilor configurate, nu a unor valori hardcodate.
- **Why:** Parametrii reali depind de statut și de răspunsurile la întrebările deschise.
- **Evidence:** Regulile obligatorii și întrebările despre penalități din planul P25 aprobat.

## D-006 · Separare strictă DEMO/REAL
- **Status:** approved
- **Rule:** Datele DEMO și REAL sunt domenii de date strict separate. Mediul activ se stabilește și se verifică pe server, nu se acceptă ca selecție de încredere din cererea clientului; toate citirile și scrierile sunt limitate la acel mediu, iar relațiile între medii sunt respinse. Datele DEMO sunt fictive și marcate vizibil `DEMO`.
- **Why:** Datele demonstrative trebuie să poată fi folosite fără amestecarea sau prezentarea lor drept date reale.
- **Evidence:** Cerința de separare din planul P25 și aprobarea explicită a separării stricte de către utilizator.

## D-007 · Autorizare pe server
- **Status:** approved
- **Rule:** Accesul la date se verifică pe server în funcție de rol și de utilizator; interfața client nu este o limită de securitate.
- **Why:** Restricțiile trebuie să se aplice chiar dacă un utilizator modifică cererea sau identificatorii trimiși.
- **Evidence:** Regulile obligatorii și etapa 8 din planul P25 aprobat.

## D-008 · Închiderea exactă a raportului de reconciliere
- **Status:** approved
- **Rule:** Raportul de reconciliere trebuie să respecte exact ecuația: sold inițial + încasări − cheltuieli = sold final.
- **Why:** Raportul financiar trebuie să se reconcilieze fără diferențe ascunse de rotunjire.
- **Evidence:** Criteriul obligatoriu pentru rapoarte din planul P25 aprobat.
