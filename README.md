# Server-X - URL Shortener Service

A C++ URL shortener service with user authentication and redirection capabilities.

## Features

- **User Registration & Login**: Secure authentication with password hashing
- **URL Shortening**: Convert long URLs into short links
- **URL Redirection**: Redirect shortened URLs to original destinations
- **Session Management**: Cookie-based user sessions
- **Web Interface**: HTML forms with Tailwind CSS styling

## Tech Stack

- **Backend**: C++23
- **Web Server**: Custom HTTP server with IPv6 support
- **Security**: OpenSSL for SHA-256 password hashing
- **Frontend**: HTML, JavaScript, Tailwind CSS
- **Build System**: CMake
- **Storage**: File-based text storage

## Project Structure

```
Server-X/
├── src/
│   ├── server.cpp              # Main server
│   ├── response.cpp            # HTTP responses
│   ├── routes/auth/            # Login/register routes
│   ├── routes/url/             # URL shortening & redirect
│   ├── utils/                  # Helper functions
│   └── database/               # User management
├── public/                     # HTML files
├── data/                       # Text file storage
└── include/                    # Headers
```

## Prerequisites

- C++ Compiler with C++23 support
- CMake 3.10+
- OpenSSL development libraries

**Ubuntu/Debian:**
```bash
sudo apt install build-essential cmake libssl-dev
```

## Setup & Run

1. **Build**
   ```bash
   mkdir build && cd build
   cmake .. && make
   ```

2. **Create data files**
   ```bash
   mkdir -p data
   touch data/database.txt data/url-mapping.txt
   ```

3. **Run**
   ```bash
   ./build/URLShortner [port]  # default port: 8080
   ```

## Usage

- **Register**: `http://localhost:8080/register`
- **Login**: `http://localhost:8080/login`
- **Shorten URLs**: `http://localhost:8080/url-shortener`

## API Endpoints

- `POST /register` - User registration
- `POST /login` - User login
- `GET /url-shortener` - URL shortener dashboard (requires login)
- `POST /url-shortener` - Create short URL (requires login)

## Data Storage

**User Database** (`data/database.txt`):
```
username,email,hashedPassword,salt,freeTrials,sessionToken,longUrls,shortUrls
```

**URL Mappings** (`data/url-mapping.txt`):
```
hash original_url
```

## Features

- SHA-256 password hashing with salt
- Session tokens for authentication
- 8-character random hash generation for short URLs
- Duplicate URL detection
- URL history tracking per user

## Notes

- Uses file-based storage (suitable for development/demo)
- IPv6 server by default
- Threaded request handling
- Custom HTTP request/response handling
