# Mannly - URL Shortener Microservice

A fast, lightweight, and containerized URL shortener service built with **Go (Fiber v3)**, **Redis**, and **Docker**.

---

## 🚀 Features

- **Shorten URLs**: Generate 6-character short identifiers for any long URL.
- **Custom Aliases**: Specify your own customized short link.
- **Link Expiration**: Configurable TTL (time-to-live) for short links (default: 24 hours).
- **Rate Limiting**: IP-based rate limiting with quota tracking in Redis.
- **Click Counter**: Tracks total redirects resolved across short URLs.
- **Containerized**: Ready to deploy with multi-stage Docker builds and Docker Compose.

---

## 🛠️ Tech Stack

- **Backend**: [Go](https://go.dev/) (Go 1.22+)
- **Web Framework**: [Fiber v3](https://gofiber.io/)
- **Cache & Database**: [Redis](https://redis.io/)
- **Containerization**: [Docker](https://www.docker.com/) & [Docker Compose](https://docs.docker.com/compose/)

---

## 📁 Project Structure

```text
mannly/
├── .data/                  # Persistent Redis data volume
├── api/
│   ├── database/           # Redis client initialization
│   ├── helpers/            # URL formatting & ID generator utilities
│   ├── routes/             # API routes (shorten, resolve)
│   ├── Dockerfile          # Multi-stage Docker build for Go API
│   ├── go.mod / go.sum     # Go dependencies
│   └── main.go             # Application entry point
├── db/
│   └── Dockerfile          # Redis container configuration
├── docker-compose.yaml     # Service orchestration (API + Redis)
└── README.md
```

---

## ⚙️ Configuration (.env)

Create a `.env` file inside the `api/` directory (or repository root depending on your execution mode) with the following values:

```env
APP_PORT=:6767
DOMAIN=localhost:6767
DB_ADDR=db:6379
DB_PASSWORD=
APP_QUOTA=10
```

> **Note**: If running Go locally outside Docker, set `DB_ADDR=localhost:6379`.

---

## 🐳 Quick Start with Docker Compose

1. **Clone the repository:**
   ```bash
   git clone https://github.com/aarjav100/mannly.git
   cd mannly
   ```

2. **Start services:**
   ```bash
   docker compose up --build
   ```

3. The API will be available at `http://localhost:6767`.

---

## 📡 API Endpoints

### 1. Shorten a URL
- **Endpoint**: `POST /api/v1`
- **Content-Type**: `application/json`

**Request Body:**
```json
{
  "urL": "https://github.com",
  "short": "my-custom-link",
  "expiry": 24
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `urL` | string | Yes | The target destination URL |
| `short` | string | No | Optional custom alias |
| `expiry` | int | No | Link expiry time in hours (default: 24) |

**Response (200 OK):**
```json
{
  "url": "https://github.com",
  "short": "localhost:6767/my-custom-link",
  "expiry": 86400000000000,
  "x-rate-remaining": 9,
  "x-rate-limit-reset": 30
}
```

---

### 2. Resolve & Redirect
- **Endpoint**: `GET /:url`
- **Example**: `http://localhost:6767/my-custom-link`
- **Behavior**: Redirects (HTTP `301 Moved Permanently`) to the original destination URL.

---

## 🌿 Git Workflow

To commit and push this change:

```bash
git add README.md
git commit -m "docs: add README with project overview and API docs"
git push -u origin feature/my-changes
```
