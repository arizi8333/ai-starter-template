# Panduan Penggunaan Blueprint

## Apa Ini?

`ai-starter-template/` adalah template project enterprise-grade. Di dalamnya ada:
- Rules (aturan kode, database, infra, QA)
- Agents (11 agent AI dengan peran masing-masing)
- Flows (6 alur kerja untuk berbagai skenario)
- Config, Memory, Contracts
- Steering file (supaya Kiro otomatis tahu semua rules)
- Project intake template (form untuk definisikan project baru)

## Cara Buat Project Baru

### Step 1: Copy Template

```bash
# Copy seluruh folder template ke project baru
cp -r ai-starter-template/ my-new-project/

# Masuk ke folder project baru
cd my-new-project/
```

Setelah copy, struktur project baru kamu:
```
my-new-project/
├── .kiro/
│   └── steering/
│       └── blueprint.md          # Kiro otomatis baca ini
├── kiro/
│   ├── agents/                   # 11 agent definitions
│   ├── flows/                    # 6 flow definitions
│   ├── memory/                   # contracts & history
│   ├── rules/                    # 6 rule files
│   └── config.yaml               # konfigurasi project
├── templates/
│   ├── project_intake.yaml       # template intake (master copy)
│   └── GUIDE.md                  # panduan ini
├── README.md
├── FEATURE_STATUS.md
└── ai_driven_starter_monorepo_blueprint_go_fe_kiro.md
```

### Step 2: Isi Project Intake

```bash
# Copy template intake ke root project
cp templates/project_intake.yaml ./project_intake.yaml

# Edit sesuai kebutuhan project kamu
# Isi: project info, tech stack, core features, user roles, auth
```

Yang perlu diisi (fokus bisnis, bukan teknis):
- **project** — nama dan deskripsi project
- **tech_stack** — pilih bahasa, framework, database
- **core_features** — deskripsikan fitur dari sisi bisnis (Kiro yang tentukan detail teknis)
- **user_roles** — siapa saja yang pakai sistem
- **auth** — butuh login atau tidak
- **roadmap** — fase-fase pengembangan (opsional tapi recommended)

### Step 3: Buka di Kiro

Buka folder `my-new-project/` di Kiro sebagai workspace.

Kiro otomatis membaca `.kiro/steering/blueprint.md` yang berisi referensi ke semua rules, agents, flows, dan contracts. Kamu tidak perlu remind Kiro secara manual.

### Step 4: Minta Kiro Setup

Chat ke Kiro:

```
Baca project_intake.yaml dan setup project ini.
```

Kiro akan:
1. Baca intake form
2. Desain database schema dari deskripsi fitur
3. Tentukan API endpoints
4. Generate folder structure sesuai tech stack
5. Buat migration files
6. Mulai implementasi core features (priority high dulu)

Semua mengikuti enterprise rules dari blueprint.

## Cara Lanjut ke Fase Berikutnya

Setelah fase 1 (MVP) selesai:

```
Lanjut ke fase 2 dari project_intake.yaml
```

Kiro akan baca section `roadmap` dan mulai kerjakan fitur-fitur di fase tersebut.

## Cara Tambah Fitur Baru (Di Luar Roadmap)

Tidak perlu edit project_intake.yaml. Langsung bilang ke Kiro:

```
Saya mau tambah fitur notifikasi email.
User bisa terima email ketika order berhasil dan ketika status order berubah.
```

Kiro akan jalankan feature_flow dan ikuti semua rules.

## Cara Refactor Code

```
Refactor module authentication supaya lebih clean dan ikuti backend_rules.md
```

Kiro jalankan refactor_flow (planner -> dev -> qa -> reviewer -> optimizer -> techwriter -> orchestrator).

## Cara Fix Bug

```
Ada bug: user bisa login tanpa password kalau email kosong
```

Kiro jalankan hotfix_flow (reviewer -> dev -> qa -> techwriter -> orchestrator).

## Cara Audit Security

```
Jalankan security audit untuk seluruh codebase
```

Kiro jalankan security_audit_flow (security -> dba -> devops -> reviewer -> techwriter -> orchestrator).

## Cara Deploy

```
Deploy ke staging environment
```

Kiro jalankan deployment_flow (devops -> security -> qa -> techwriter -> orchestrator).

## Cara Migrasi Database

```
Saya perlu tambah kolom phone_number di tabel users
```

Kiro jalankan migration_flow (planner -> dev -> dba -> reviewer -> qa -> techwriter -> orchestrator).

## Tips

- Deskripsikan fitur dari sudut pandang user/bisnis, bukan teknis
- Semakin jelas deskripsi, semakin tepat output Kiro
- Mulai dengan 2-3 fitur priority high saja di fase 1
- Gunakan `notes` di intake untuk konteks yang tidak bisa ditangkap field structured
- Kiro akan selalu ikuti rules — kalau ada yang melanggar, QA akan fail dan feedback loop aktif
- Setiap perubahan di-track di FEATURE_STATUS.md dan quality_history.json
