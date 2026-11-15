# Architecture — web2-lab2026

<!-- graffino-agentic:begin v1 -->
> Keep this page grounded in project files. The code wins when this page is stale.
<!-- graffino-agentic:end -->

## Overview
Approved target architecture for P25; implementation has not started. See [P25 implementation plan](../P25-PLAN.md). The application will use Next.js with TypeScript, PostgreSQL through Prisma, and server-side services for financial operations. Code takes precedence over this plan if the implementation later differs; such differences must be reviewed against the approved decisions.

## Parts
- Next.js user interface for association administration, reconciliation, reporting, and member access.
- Server-side services for authentication and authorization, statement import, matching, allocations, penalties, and reports.
- PostgreSQL data store accessed through Prisma.
- Isolated bank statement parsers, one per supported format.
- Demo data generator for 20 apartments, three months of history, and two fictional bank formats.

## Data flow
An authorized user uploads a statement; a format-specific parser validates it and returns a preview; the user confirms; a transactional, idempotent import stores valid transactions in the active data environment. Matching proposes explainable associations, uncertain cases go to manual review, and confirmed allocations feed configured penalty calculations and reports. Member requests are scoped to that member on the server.

## State and storage
PostgreSQL stores users, roles, apartments, members, bank accounts, statements, transactions, obligations, allocations, matching rules, penalty rules, and audit events. Monetary values use integer bani. DEMO and REAL are strictly separated: the server establishes the active environment, scopes reads and writes, and rejects cross-environment relations, as required by D-006.

## Configuration
Penalty behavior and matching rules are configured rather than hardcoded. Real bank formats and statutory parameters remain open under Q-004–Q-009. Demo fixtures are fictional and visibly labeled DEMO.

## Build, deploy, runtime
Planned stack and quality tools are listed in D-001. Runtime commands and versions remain unknown under Q-002; release and deployment process remains unknown under Q-003. No deployment is included in the approved plan.

## Boundaries
Authorization and data-environment checks happen on the server. Bank parsers are isolated by format. API route details are specified in [P25 implementation plan](../P25-PLAN.md) and will be refined when implementation begins. Real-world matching and penalty parameters remain blocked by the open questions.
