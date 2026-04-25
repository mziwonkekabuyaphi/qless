# QLess API Documentation

## Base URL
`https://api.qless.co.za/api/v1`

## Authentication

### Login
```http
POST /auth/login
Content-Type: application/json

{
    "email": "user@example.com",
    "password": "password",
    "device_name": "web_app"
}
