# Task Management API

REST API untuk mengelola project dan task tim, dibangun dengan Laravel dan
Laravel Sanctum. Menyediakan autentikasi berbasis token, proteksi route
lewat middleware, dan struktur resource yang konsisten untuk project dan
task yang saling berelasi.

## Fitur

- Registrasi & login dengan token akses (Laravel Sanctum)
- Middleware `auth:sanctum` melindungi seluruh endpoint sensitif
- CRUD penuh untuk **Projects** (`apiResource`)
- CRUD **Tasks** yang bersarang di bawah Project (nested resource)
- Kontrak respons JSON yang konsisten di seluruh endpoint
- Sudah diuji end-to-end lewat Postman

## Tech Stack

| Komponen | Teknologi |
|---|---|
| Framework | Laravel 12 |
| Bahasa | PHP 8.2 |
| Autentikasi | Laravel Sanctum |
| Database | MySQL |
| Testing manual | Postman |

## Referensi Endpoint

### Publik

| Method | Endpoint | Deskripsi |
|---|---|---|
| POST | `/api/register` | Daftarkan akun baru |
| POST | `/api/login` | Tukar kredensial dengan token akses |

### Terproteksi (butuh header `Authorization: Bearer {token}`)

| Method | Endpoint | Deskripsi |
|---|---|---|
| POST | `/api/logout` | Cabut token akses aktif |
| GET | `/api/me` | Ambil data user yang sedang login |
| GET | `/api/projects` | Daftar semua project |
| POST | `/api/projects` | Buat project baru |
| GET | `/api/projects/{project}` | Detail satu project |
| PUT/PATCH | `/api/projects/{project}` | Perbarui project |
| DELETE | `/api/projects/{project}` | Hapus project |
| GET | `/api/projects/{project}/tasks` | Daftar task dalam satu project |
| POST | `/api/projects/{project}/tasks` | Tambah task ke project |
| GET | `/api/tasks/{task}` | Detail satu task |
| PUT/PATCH | `/api/tasks/{task}` | Perbarui task |
| DELETE | `/api/tasks/{task}` | Hapus task |

## Contoh Request & Response

**Login**

```http
POST /api/login
Content-Type: application/json
Accept: application/json

{
  "email": "admin@example.com",
  "password": "password"
}
```

Response `200 OK`:

```json
{
  "message": "Login successful",
  "user": {
    "id": 1,
    "name": "Bima Maulana",
    "email": "admin@example.com",
    "role": "admin"
  },
  "token": "2|R9AGYR7Y..."
}
```

Token yang didapat dipakai untuk mengakses endpoint terproteksi lewat header:

```
Authorization: Bearer 2|R9AGYR7Y...
```

## Instalasi & Menjalankan Secara Lokal

### Prasyarat

- PHP >= 8.2
- Composer
- MySQL
- Laragon / XAMPP / server lokal PHP lainnya

### Langkah

```bash
git clone https://github.com/bimaamaulana/task-management-api.git
cd task-management-api
composer install
cp .env.example .env
php artisan key:generate
```

Buka file `.env`, sesuaikan konfigurasi database:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=task_management_api
DB_USERNAME=root
DB_PASSWORD=
```

Jalankan migrasi database:

```bash
php artisan migrate
```

Jalankan server:

```bash
php artisan serve
```

API akan tersedia di `http://127.0.0.1:8000/api`.

## Testing

Seluruh endpoint sudah diuji manual menggunakan Postman, mencakup alur
register → login → akses endpoint terproteksi dengan token → logout.

## Catatan Deployment

Project ini adalah backend API murni dan membutuhkan server yang mendukung
PHP serta MySQL (misalnya Railway, Render, atau shared hosting berbasis
PHP). GitHub Pages tidak dapat menjalankan aplikasi ini karena hanya
melayani file statis.

## Author

**Bima Maulana**
Dibuat sebagai bagian dari persiapan sertifikasi kompetensi Database
Administrator (skema LSP/BNSP).
