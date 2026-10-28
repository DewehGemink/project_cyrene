# Elysia Chat

CLI chat berbasis Python dengan persona roleplay terinspirasi Elysia. Mendukung Google Gemini dan API router yang kompatibel dengan format OpenAI. Konfigurasi dibaca dari file `.env` di folder yang sama dengan script, termasuk saat script dijalankan dari folder lain.

## Menjalankan

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
# Isi credential provider di file .env
python elysia_chat.py
```

## Menggunakan API router

Untuk layanan yang kompatibel dengan OpenAI Chat Completions, ubah `.env`:

```dotenv
ELYSIA_PROVIDER=openai-compatible
ELYSIA_BASE_URL=https://api-cdn.deploy.com/api/v1
ELYSIA_API_KEY=api_key_dari_provider
ELYSIA_MODEL=deepseek-ai/deepseek-v4.1-flash-1017
```

Ganti API key dengan key milikmu. `ELYSIA_BASE_URL` adalah root endpoint yang diberikan
layanan; client akan menambahkan path API yang diperlukan. Nilai contoh di atas mengikuti
snippet Deploy.com yang kamu kirim.
Untuk provider lain yang memakai protokol OpenAI-compatible, cukup ganti base URL, key,
dan model. Gemini tetap dapat dipilih dengan `ELYSIA_PROVIDER=gemini`.

Saat startup, aplikasi menampilkan provider, model, dan base URL aktif (API key tidak pernah ditampilkan), supaya konfigurasi yang terbaca bisa langsung dicek.

Untuk mengurangi waktu respons, aplikasi mengirim maksimal 6 giliran terakhir ke model,
memakai streaming, dan membatasi jawaban hingga 350 token. Nilai ini dapat diatur melalui
`ELYSIA_MAX_HISTORY_TURNS`, `ELYSIA_MAX_OUTPUT_TOKENS`, `ELYSIA_API_TIMEOUT`, dan
`ELYSIA_API_MAX_RETRIES` di `.env`. Retry default dimatikan agar error jaringan cepat
terlihat; naikkan ke `1` jika kamu lebih mengutamakan retry otomatis.

> **Catatan**: Gunakan perintah `python` (bukan `py`) saat `.venv` aktif, karena di beberapa sistem Linux alias `py` dapat mengarah ke Python sistem di luar virtual environment.

Perintah dalam chat:

- `/reset` menghapus konteks percakapan
- `/save` menyimpan riwayat ke `conversation.json`
- `/quit` menyimpan lalu keluar

Prompt persona berada di `SYSTEM_PROMPT` dalam `elysia_chat.py`, sehingga mudah disesuaikan untuk kadar humor, panggilan, atau bahasa yang kamu suka.
Karakter ini adalah roleplay non-resmi; aplikasi tidak menyalin dialog kanon.
