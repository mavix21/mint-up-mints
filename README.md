# Mint Up! MINTs

Mint Up! is a community-focused event management platform. This public repository will contain the attendance credential contracts and a separate website for viewing and verifying MINTs.

A MINT is a planned non-transferable attendance credential issued with a community's authorization. Communities will be able to use these credentials to recognize participation and offer benefits at later events.

## Current status

**Scaffolding only. No MINT product functionality is implemented.**

| Component | Present now | Planned |
| --- | --- | --- |
| Rust contracts | Cargo workspace and empty `mints` crate generated with Stellar CLI | Attendance MINT contract, permissions, issuance and invalidation |
| Verification website | Next.js empty template with a Hello world page | MINT lookup, live Testnet checks and independent image display |
| Deployment records | Testnet manifest with an empty contract map | Actual contract addresses and deployment versions |
| Documentation | Project overview and scope | Architecture, permissions, metadata schema and setup evidence |

The repository has no deployed contracts, verification service, wallet integration or IPFS integration. Scaffold checks only confirm that the starter project can be built. They do not demonstrate any Instaward deliverable.

Mint Up!'s existing event management application is maintained in a separate private repository. Its code and user data are outside this repository.

## Proposed Instaward scope

The proposal covers **three deliverables, USD 5,000 and 30 calendar days**. All blockchain operations are planned for **Stellar Testnet**. This README describes proposed work, not a completed sprint or a funding award.

### 1. Community-authorized attendance MINTs

Integrate Smart Account Kit and OpenZeppelin's existing smart-account implementation. Each enabled community will have its own account, separate from the personal accounts that receive MINTs. One representative with a passkey is required. Additional representatives are optional.

Organizers will enable MINTs from Event Management. One passkey confirmation will authorize the event collection, its issuance limit, deadline and service permissions. Creating or publishing an event will not require a blockchain signature.

Extend existing OpenZeppelin Stellar NFT components for non-transferable attendance credentials. Each collection will have one issuing community. Record the event, issuer, owner, issuance time and validity. Publish metadata and images through IPFS and store their reference in the contract.

The community will control issuance and correction permissions and can revoke them separately. Mint Up! will check attendance and account control before issuing. Tests will cover permissions, key replacement, duplicate claims, replay protection, revocation and invalidation.

The design retains trust in the attendance process. A compromised service key can act within its authorized limits. Recovery requires an accessible registered passkey. These limits and operating procedures will be documented.

### 2. Public MINT verification website

Build a separate open-source website that reads directly from Stellar Testnet and public IPFS files. Users will look up a MINT by contract and token ID or list MINTs held by an account.

The website will show the issuing community, event, owner, issuance time, validity, name, description and image. Viewing will work independently of Mint Up!'s login, API, database and media storage.

Evidence will show an active credential with the correct image and metadata, plus an invalidated credential with its updated status. Public file links and a short network-request capture will demonstrate independent retrieval.

### 3. MINT-gated free tickets and pilot

Add one required MINT collection to existing free ticket options in the private Mint Up! application. Organizers will describe an additional benefit in plain text. General and Holders tickets can coexist. The benefit will be delivered manually.

Ticket acquisition will check account control, an active MINT, ticket capacity and duplicate allocations. The MINT stays in the attendee's account. The restricted option will use free self-registration.

The proposed pilot uses two real events with dev3pack Perú, where Mint Up! team members are Core members. Five participants will receive attendance MINTs, obtain holder tickets at the second event, check in and receive the stated benefit. Organizers will choose that benefit and confirm delivery capacity before registration opens.

This integration remains in the private application. Public evidence will show its behavior without publishing private application code or participant identifiers.

## Schedule and budget

| Period | Planned result |
| --- | --- |
| Days 1–7 | Account compatibility, permissions and pilot coordination |
| Days 8–14 | Account integration by Day 10, event activation and issuance work |
| Days 15–21 | Deliverable 1 by Day 16, Event A on Day 18, Event B registration by Day 21 |
| Days 22–30 | Verification website by Day 24, Event B on Day 26, evidence and acceptance by Day 30 |

| Work | Budget |
| --- | ---: |
| Deliverable 1 engineering — 150 hours | USD 3,000 |
| Deliverable 2 engineering — 45 hours | USD 900 |
| Deliverable 3 engineering — 45 hours | USD 900 |
| Interface design | USD 200 |
| **Total** | **USD 5,000** |

Engineering totals 240 team hours at USD 20 per hour. Services, event logistics and participant benefits are funded outside this request.

Each deliverable will have a short video and a concise evidence PDF. This repository will hold the public contracts, contract tests, website, metadata schema and run instructions as they are developed.

## Scope boundaries

The sprint excludes Mainnet operation, paid gated tickets, requirements spanning multiple collections, joint community signatures, Quest credentials, advanced recovery, inventory integration, bulk indexing and additional pilots.

Testnet records are temporary and may be reset. The pilot will maintain contract data and IPFS files through review. A reset requires redeployment and fresh test credentials. Saved media and evidence preserve the completed demonstration.

## Repository layout

```text
apps/
  verifier/             # Next.js website scaffold
contracts/
  mints/                # Empty Rust contract crate
deployments/
  testnet.json          # No deployment addresses yet
docs/                   # Documentation placeholder
Cargo.toml              # Rust workspace
pnpm-workspace.yaml     # JavaScript workspace
```

## Local development

Use Node.js 20.9 or later, pnpm 12.16.1, Rust, and Stellar CLI. Install the `wasm32v1-none` Rust target for contract builds.

```sh
pnpm install --frozen-lockfile
pnpm dev
```

The website runs at http://localhost:3000 and displays the generated starter page.

```sh
pnpm lint
pnpm typecheck
pnpm build
cargo fmt --all --check
cargo check --workspace --locked
stellar contract build
```

These commands do not deploy contracts or create accounts. The empty Rust crate has no contract methods or product tests.

## Scaffold provenance

The initial structure was generated with official tools:

- `stellar contract init <project-path> --name mints` using Stellar CLI 23.1.4.
- `pnpm init` for the root JavaScript package.
- `pnpm create next-app@latest apps/verifier --empty --ts --eslint --app --src-dir --use-pnpm --disable-git --skip-install` with the optional Tailwind, React Compiler, cache-components and agent-feedback options disabled.

At scaffold creation, npm's latest stable Next.js version was **16.4.5**. The package manifest pins that version. The root pnpm workspace replaces the generator's nested workspace. The generated Rust greeting method and its sample test were removed to leave an empty crate. No MINT logic was added.

Smart Account Kit and OpenZeppelin integration will be selected and versioned during the sprint. Their presence in the scope does not mean they are integrated in this scaffold.

References: [Next.js CLI](https://nextjs.org/docs/app/api-reference/cli/create-next-app), [Stellar project initialization](https://developers.stellar.org/docs/build/guides/dapps/soroban-contract-init-template).

## License

Mint Up!'s code in this repository is licensed under MIT. Dependencies retain their own licenses.
