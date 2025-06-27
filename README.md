# Server-X - URL Shortener Service

A high-performance URL shortener service built with C++ that provides user authentication, URL shortening, and redirection capabilities.

## Features

- **User Authentication**: Registration and login system with secure password hashing
- **URL Shortening**: Convert long URLs into short, manageable links
- **URL Redirection**: Seamless redirection from shortened URLs to original destinations
- **Session Management**: Secure cookie-based session handling
- **User Dashboard**: View URL history and manage shortened links
- **Responsive Web Interface**: Clean, modern UI built with Tailwind CSS

## Tech Stack

- **Backend**: C++23
- **Web Server**: Custom HTTP server with IPv6 support
- **Security**: OpenSSL for password hashing and secure token generation
- **Frontend**: HTML, JavaScript, Tailwind CSS
- **Build System**: CMake
- **Database**: File-based storage system

## Project Structure

```
Server-X/
├── src/
│   ├── server.cpp              # Main server implementation
│   ├── response.cpp            # HTTP response handling
│   ├── routes/
│   │   ├── auth/               # Authentication routes
│   │   ├── url/                # URL shortening routes
│   │   └── user/               # User management routes
│   ├── utils/                  # Utility functions
│   ├── database/               # Database operations
│   └── request-parser/         # Request parsing utilities
├── public/                     # Static web files
│   ├── login.html
│   ├── register.html
│   └── url-shortner.html
├── data/                       # Data storage
│   ├── database.txt            # User database
│   └── url-mapping.txt         # URL mappings
└── include/                    # Header files
```

## Prerequisites

- **C++ Compiler**: GCC 11+ or Clang 13+ (C++23 support required)
- **CMake**: Version 3.10 or higher
- **OpenSSL**: Development libraries
- **Git**: For cloning the repository

### Installing Dependencies

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt install build-essential cmake libssl-dev git
```

**CentOS/RHEL/Fedora:**
```bash
sudo yum install gcc-c++ cmake openssl-devel git
# or for newer versions:
sudo dnf install gcc-c++ cmake openssl-devel git
```

**macOS:**
```bash
brew install cmake openssl git
```

## Installation & Setup

1. **Clone the Repository**
   ```bash
   git clone <repository-url>
   cd Server-X
   ```

2. **Build the Project**
   ```bash
   mkdir build
   cd build
   cmake ..
   make
   ```

3. **Create Data Directory**
   ```bash
   mkdir -p data
   touch data/database.txt
   touch data/url-mapping.txt
   ```

4. **Run the Server**
   ```bash
   ./build/URLShortner [port]
   ```
   Default port is 8080 if not specified.

## Usage

### Starting the Server

```bash
# Run on default port (8080)
./build/URLShortner

# Run on custom port
./build/URLShortner 3000
```

### Accessing the Service

- **Registration**: `http://localhost:8080/register`
- **Login**: `http://localhost:8080/login`
- **URL Shortener**: `http://localhost:8080/url-shortener`
- **Shortened URLs**: `http://localhost:8080/vardaan.ly/{hash}`

## API Endpoints

### Authentication

- **POST** `/register` - User registration
  - Body: `username`, `email`, `password`, `confirmPassword`
  - Response: Registration success/failure

- **POST** `/login` - User login
  - Body: `email`, `password`
  - Response: Login success with session cookie

### URL Operations

- **GET** `/url-shortener` - Access URL shortener dashboard
  - Requires: Valid session cookie
  - Response: HTML dashboard with URL history

- **POST** `/url-shortener` - Shorten a URL
  - Body: `url` (original URL to shorten)
  - Response: Shortened URL

- **GET** `/vardaan.ly/{hash}` - Redirect to original URL
  - Response: 301 redirect to original URL

### User Management

- **GET** `/get-user` - Get user information
- **GET** `/get-all-users` - Get all users (admin)

## Security Features

- **Password Hashing**: SHA-256 with salt
- **Session Management**: Secure HTTP-only cookies
- **Input Validation**: Email format validation and sanitization
- **HTTPS Ready**: SSL/TLS support with proper headers

## File Storage Format

### User Database (`data/database.txt`)
```
username,email,hashedPassword,salt,freeTrialsLeft,sessionToken,urlHistoryLong,urlHistoryShort
```

### URL Mapping (`data/url-mapping.txt`)
```
hash original_url
```

## Configuration

### Default Settings
- **Port**: 8080
- **Session Cookie Lifetime**: 30 days
- **Hash Length**: 8 characters
- **Free Trials per User**: 3

### Customization
Modify constants in source files:
- `src/utils/short-url-generator.cpp` - Hash generation settings
- `src/routes/auth/login.cpp` - Session cookie settings
- `src/database/user.cpp` - User trial limits

## Development

### Adding New Routes

1. Create route handler in appropriate directory under `src/routes/`
2. Add function declaration to corresponding header file
3. Register route in `src/server.cpp`
4. Update CMakeLists.txt if adding new source files

### Building for Development

```bash
# Enable debug mode
cmake -DCMAKE_BUILD_TYPE=Debug ..
make

# Enable verbose output
make VERBOSE=1
```

## Known Limitations

- File-based storage (not suitable for high-scale production)
- Single-threaded request handling per connection
- No user role management
- Limited error handling for malformed requests

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Commit changes: `git commit -am 'Add feature'`
4. Push to branch: `git push origin feature-name`
5. Submit a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Troubleshooting

### Common Issues

**Build Errors:**
- Ensure C++23 compiler support
- Verify OpenSSL development libraries are installed
- Check CMake version (3.10+ required)

**Runtime Errors:**
- Verify data directory exists and is writable
- Check port availability (default 8080)
- Ensure proper file permissions

**Connection Issues:**
- Server supports IPv6 by default
- Try accessing via `http://[::1]:8080` for IPv6
- Use `http://localhost:8080` for IPv4

### Debug Mode

Enable debug output by adding `-DDEBUG` to compile flags in CMakeLists.txt.

## Performance Notes

- Handles concurrent connections using threading
- File I/O operations are synchronous
- Memory usage scales with number of stored URLs and users
- Recommended for development and small-scale deployments

## Future Enhancements

- Database integration (PostgreSQL/MySQL)
- Redis caching for improved performance
- User dashboard with analytics
- API rate limiting
- Custom short URL aliases
- Bulk URL shortening
- URL expiration dates
