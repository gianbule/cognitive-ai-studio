# Riset Kebutuhan Produk & Benchmark UX AI Chat

> Proyek: **Cognitive AI Studio**  
> Fase: 1 – Discovery, Design System, dan Fondasi Fullstack  
> Task: Riset kebutuhan produk dan benchmark UX AI chat via Brave Web Search, Fetch & Webpage Reader; simpan insight ke Knowledge Graph Memory.

## 1. Ringkasan Eksekutif

Cognitive AI Studio ditargetkan sebagai platform AI percakapan multi-agen dengan streaming response dan perpustakaan prompt kustom untuk kreator konten, developer, dan peneliti AI. [file:1] Benchmark UX dari produk AI chat terkemuka (Perplexity, Claude) menunjukkan bahwa pengalaman yang dapat dipercaya dan efisien dibangun di atas: transparansi sumber, kontrol riset yang jelas, feedback proses yang terbaca, dan desain yang memprioritaskan aksesibilitas serta ergonomi mobile. [web:2][web:3][web:4]

## 2. Kebutuhan Produk Inti (dari MASTERPROMPT)

Berdasarkan spesifikasi fungsional dalam `MASTERPROMPT.md`, kebutuhan produk inti meliputi:

1. **Interface Chat Streaming** dengan format Markdown dan syntax highlighting kode. [file:1]
2. **Manajemen Percakapan Berkelanjutan**: history thread, perekaman sesi, dan kemampuan melanjutkan konteks. [file:1]
3. **Prompt Library** siap pakai dengan kategori, sistem favorit, dan pencarian. [file:1]
4. **Pelacak Penggunaan Token Realtime** dan metrik efisiensi biaya API. [file:1]
5. **Pengaturan Parameter Model AI**: temperature, system persona, top-p, dan preset. [file:1]
6. **Ekspor Percakapan** ke PDF, Markdown, dan tautan publik yang dapat dibagikan. [file:1]

Arsitektur teknis yang ditargetkan adalah fullstack dengan React 19 + Vite (TypeScript) di frontend, React Native + Expo di mobile, Tailwind CSS + Shadcn UI, Zustand untuk state, Node.js + Express di backend, serta PostgreSQL dengan Drizzle/Prisma. [file:1]

## 3. Benchmark UX AI Chat

### 3.1 Perplexity: Transparansi Sumber dan Sitasi

Perplexity dirancang sebagai mesin jawaban berbasis AI yang menggabungkan pencarian web realtime dengan ringkasan yang disertai sitasi bernomor. [web:2] Setiap jawaban menyertakan tautan ke sumber asli sehingga pengguna dapat memverifikasi klaim dan mengeksplorasi bukti lebih lanjut. [web:2] Beberapa domain juga diberi label khusus (misalnya "Government") sebagai bagian dari proses review sumber. [web:2]

**Implikasi untuk Cognitive AI Studio:**

- Setiap jawaban AI harus menyertakan **sitasi inline bernomor** yang dapat diklik/hover.
- Panel sumber harus menampilkan: judul halaman, domain, kutipan relevan, dan tautan eksternal.
- Sitasi harus dapat diakses keyboard dan screen reader (fokus terlihat, label ARIA yang jelas).

### 3.2 Claude: Kontrol Web Search dan Indikator Aktivitas

Claude mendukung **web search** yang dapat diaktifkan/dinonaktifkan oleh pengguna (pada level workspace atau per sesi) untuk mendapatkan informasi terkini. [web:3] Saat search aktif, Claude menampilkan indikator aktivitas sebelum menghasilkan jawaban, dan respons yang dihasilkan mencakup sitasi serta sumber lanjutan. [web:3]

**Implikasi untuk Cognitive AI Studio:**

- Sediakan kontrol eksplisit **"Web research"** di composer atau settings thread.
- Tampilkan indikator status: `thinking` → `searching` → `reading_sources` → `generating`.
- Berikan pesan status yang ramah screen reader (`aria-live="polite"`) agar pengguna memahami apa yang sedang dilakukan sistem.

### 3.3 Claude Research: Riset Berantai Multi-Sudut

Fitur **Research** pada Claude (untuk pengguna berbayar) menjalankan beberapa pencarian berantai, menelusuri berbagai sudut pertanyaan, lalu menyajikan jawaban bercitasi yang terstruktur. [web:4] Proses ini dirancang untuk analisis mendalam dalam waktu singkat, dengan penjelasan bahwa sistem melakukan iterasi atas sub-pertanyaan. [web:4]

**Implikasi untuk Cognitive AI Studio:**

- Untuk mode "Riset mendalam", tampilkan progress ringkas: jumlah sumber diproses, sumber aktif, dan tombol cancel.
- Hasil riset sebaiknya terstruktur: ringkasan eksekutif, poin-poin kunci, dan daftar sumber terkelompok.
- Hindari UI yang terlalu bising; fokus pada informasi yang membantu pengguna menilai kualitas riset.

### 3.4 Analisis URL dan Dokumen

Claude dapat melakukan **web fetch terhadap URL langsung**, namun dokumen panjang dapat memakan banyak context/kuota. [web:3] Ini menuntut UX yang jujur tentang biaya dan batasan, serta memberikan kontrol kepada pengguna.

**Implikasi untuk Cognitive AI Studio:**

- Saat pengguna melampirkan URL, tampilkan status fetch, batas ukuran/timeout, dan ringkasan sumber.
- Sediakan error action yang dapat dicoba ulang dan penjelasan dampak terhadap penggunaan token.
- Jangan menyembunyikan biaya atau dampak pemakaian context dari pengguna.

## 4. Prinsip UX yang Direkomendasikan

Berdasarkan benchmark di atas dan standar UI/UX Pro Max dalam `MASTERPROMPT.md`, prinsip UX berikut direkomendasikan:

1. **Evidence-first**: Setiap klaim AI harus dapat ditelusuri ke sumber (sitasi inline, panel sumber). [web:2]
2. **Transparent process**: Status proses (thinking/searching/reading/generating) harus terlihat dan terbaca. [web:3][web:4]
3. **User control**: Kontrol eksplisit untuk web research, cancel generation, dan pengaturan parameter model. [web:3][file:1]
4. **Accessibility by default**: Kontras WCAG AA, target sentuh ≥44px, fokus keyboard jelas, label ARIA lengkap. [file:1]
5. **Mobile-first ergonomics**: Layout responsif, safe-area insets, dan interaksi sentuh yang presisi. [file:1]
6. **Honest cost**: Tampilkan dampak penggunaan token dan biaya secara realtime, terutama untuk fitur berat seperti riset mendalam atau analisis URL. [file:1][web:3]

## 5. State & Interaksi UI yang Diperlukan

Berikut state dan interaksi minimal yang harus didukung oleh komponen chat:

- **State streaming**: `idle` | `thinking` | `searching` | `reading_sources` | `generating` | `complete` | `error` | `cancelled`.
- **Feedback proses**: Skeleton/loading halus, indikator aktivitas, dan pesan status untuk screen reader.
- **Sitasi**: Sitasi bernomor inline, panel sumber dengan detail, dan tautan eksternal.
- **Kontrol**: Tombol stop generation, retry, toggle web research, dan akses pengaturan parameter.
- **Aksesibilitas**: `aria-live` untuk status, fokus keyboard terlihat, kontras memadai, target sentuh ≥44px.

## 6. Insight untuk Knowledge Graph

Berikut entitas dan relasi kunci yang sebaiknya disimpan ke Knowledge Graph Memory:

```text
Project: Cognitive AI Studio
  ├─ targets → Audience: Content Creators
  ├─ targets → Audience: Software Developers
  ├─ targets → Audience: AI Researchers
  ├─ requires → Feature: Streaming AI Chat
  ├─ requires → Feature: Conversation Threads
  ├─ requires → Feature: Prompt Library
  ├─ requires → Feature: Usage and Cost Metrics
  └─ requires → Feature: Export and Public Sharing

UX Decision: Evidence-first chat
  ├─ informed_by → Perplexity: Inline numbered citations
  ├─ informed_by → Claude: Search activity indicator
  ├─ informed_by → Claude: Explicit web-search control
  └─ requires → UI State: source-loading / source-success / source-error

UX Decision: Research progress visibility
  ├─ requires → State: thinking
  ├─ requires → State: searching
  ├─ requires → State: reading_sources
  ├─ requires → State: generating
  ├─ requires → Action: cancel_generation
  └─ requires → Accessibility: aria-live status announcement
```

## 7. Referensi

1. Perplexity Help Center – "How does Perplexity work?" [web:2]
2. Claude Help Center – "Enable and use web search" [web:3]
3. Claude Help Center – "Use research on Claude" [web:4]
4. MASTERPROMPT.md – Spesifikasi proyek Cognitive AI Studio [file:1]
