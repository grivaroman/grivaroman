# Griva Roman

**Backend & full-stack engineer.** I build production systems end to end: Python and Node.js APIs, Postgres schemas, real-time layers, Docker/K8s deploys — plus the React and Flutter clients that consume them.

Most of my work ships to real users in marketplace, casting, streaming and media-generation products.

- 🛠 **Stack:** Python · FastAPI · SQLAlchemy · Node.js · Express · Prisma · PostgreSQL · Redis · Socket.IO · React · Flutter · Docker · Kubernetes
- 🔭 **Currently:** building AI video-generation pipelines and multi-tenant SaaS backends
- 📫 **Reach me:** [grivaroman1137@gmail.com](mailto:grivaroman1137@gmail.com) · [Telegram](https://t.me/playman666) · [LinkedIn](https://www.linkedin.com/in/роман-грива-3438b3333)

---

## Open source

Code you can read, with tests you can run.

### 👁 [person-id-live](https://github.com/grivaroman/person-id-live) — real-time person identification
A face anchors a name to a ByteTrack id; when the person turns away, a fused body score carries it — ReID 50%, clothing colour 30%, body proportions 20%. The interesting part isn't the models (all off-the-shelf) but the composition: three metrics that don't share a scale mapped onto one 0–1 axis, a missing cue redistributing its weight instead of contributing a false zero, Intersection-over-Face binding instead of centre-point containment so a crowd doesn't hand a face to the wrong track. Ships a FAR/FRR calibrator that reports thresholds on the exact scale the runtime compares against.
`Python · PyTorch · FastReID · InsightFace · MediaPipe · YOLOv8 · OpenCV`

### 🎞 [unsub](https://github.com/grivaroman/unsub) — erase burned-in subtitles without inpainting
Inpainting invents pixels and smears. These pixels aren't missing — they're in the neighbouring frames, just at different coordinates. ORB features detected everywhere except the subtitle band, neighbours warped in by RANSAC similarity transform, per-pixel median over the donors whose own band was elsewhere. RANSAC doubles as a cut detector: too few inliers means a different shot, so no scene bleeds into another. On the synthetic benchmark, error on text pixels drops 88.4 → 6.1 against a codec floor of 4.3.
`Python · OpenCV · NumPy`

### 🧠 [neural-intent-bot](https://github.com/grivaroman/neural-intent-bot) — a classifier built from scratch on numpy
No scikit-learn, no PyTorch. Char n-gram TF-IDF vectorizer, a 64→32→N MLP, every gradient written out by hand, weights serialised to JSON, online learning from user corrections. Character n-grams rather than words because the input is short, misspelled, inflected Russian chat text where word tokens hit an OOV wall. ~400 lines of ML; the Telegram bot that uses it is an example, not the point.
`Python · NumPy`

### 🎮 [gameclubs_backend](https://github.com/grivaroman/gameclubs_backend) — seat booking for computer clubs
A booking backend where the same data serves three different audiences: a client books a seat, a club owner runs their own CRM over the bookings, and a superadmin moderates club applications. One async SQLAlchemy layer under all three, JWT kept in HTTPOnly cookies rather than local storage, Jinja UI and a headless JSON API over the same routers, Alembic migrations and a seeded local stack that comes up with one `docker-compose up`.
`Python · FastAPI · SQLAlchemy · PostgreSQL · Alembic · Docker`

---

## Commercial work

Closed source — described here, with the shipped apps linked where they exist.

### 🎬 Casting & KinoBaza — casting platform backend
📱 Live: [App Store](https://apps.apple.com/kz/app/kinobaza/id6761676312) · [Google Play](https://play.google.com/store/apps/details?id=kz.kinobaza.app)

Marketplace connecting actors, models and production crews. The parts I'd want to talk through: a notification drainer where one CTE both claims and marks a batch under `FOR UPDATE ... SKIP LOCKED`, so two replicas take disjoint rows and nobody gets a duplicate push; a per-project `pg_try_advisory_xact_lock` guarding AI analysis against double submission; and a three-way merge that re-applies analysis without overwriting user edits, raising divergences as conflicts instead of silently winning. Plus keyset feed pagination on partial indexes, Redis-backed Socket.IO, S3 media with BlurHash, Firebase OTP and Apple Sign-In.
`Node.js · Prisma · PostgreSQL · Redis · gRPC · Docker`

### 🎉 EventsAI — event-services marketplace
📱 Live: [App Store](https://apps.apple.com/kz/app/eventsai/id6787691928) · [Google Play](https://play.google.com/store/apps/details?id=kz.eventai)

Booking backend for event vendors. Multi-role JWT auth, booking and scheduling engine, budget-aware package assembly that splits candidates into price terciles and allocates greedily from the cheapest categories so one expensive pick can't eat the budget, image pipeline on `sharp` + S3, Swagger-generated API docs.
`Node.js · Express · Prisma · PostgreSQL · Redis · Socket.IO`

### 📺 LOLDOL — streaming platform
Microservices backend for video streaming. An HLS ABR ladder whose rungs are defined by the short side, with the long side derived from the source aspect ratio — vertical and horizontal sources each encode to their own geometry, no letterboxing, no upscaling — and closed GOPs with keyframes aligned to segment boundaries so the renditions are actually switchable.
`Node.js · microservices · gRPC · ffmpeg · Kubernetes`

### 🎥 video_pipeline — AI video generation service
FastAPI service turning a script into a finished video. What I'm proudest of is the instrumentation around money and quality: retry budgets derived from the measured cost of an attempt (balance delta, not a price list that silently goes stale), money guards raised as `BaseException` so best-effort `except Exception` blocks can't swallow them, regenerated panels marking downstream work `stale` rather than cascading a rebuild, and an A/V sync probe with asymmetric thresholds per ITU-R BT.1359 that distinguishes a fixed offset from clock drift.
`Python · FastAPI · SQLAlchemy · ffmpeg · MCP`

---

## How I work

- **Contracts before code.** APIs get documented and versioned; clients are written against the spec, not the implementation.
- **Architecture that holds.** Dependency direction enforced by tooling, not good intentions — so the core stays testable when adapters change.
- **Measure, then tune.** A threshold picked by eye is a threshold nobody can defend. If a number governs behaviour, something in the repo should compute it.
- **Deploy is part of the feature.** Dockerfiles, Helm charts and K8s manifests live in the repo alongside the code.
- **Decisions get written down.** Trade-offs are recorded where the next person will actually read them — including the ones that didn't work out.
