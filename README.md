# GoServer - Proxy Re-Encryption Healthcare System

A Go-based distributed server system implementing proxy re-encryption for secure healthcare data management. This system allows patients to encrypt their medical files and securely share access with authorized healthcare professionals through a proxy server.

## 🏗️ Architecture

The system consists of three main server components:

1. **Patient Server** (Port 8083) - Manages patient data and file encryption
2. **Proxy Server** (Port 8082) - Handles re-encryption keys and transforms encrypted data
3. **User Server** (Port 8084) - Manages healthcare professional access and decryption

## 📁 Project Structure

```
goserver/
├── Encryptionlogic/       # Core encryption/decryption logic
├── serverpatient/         # Patient server implementation
├── serverproxy/           # Proxy server implementation
├── serveruser/            # Healthcare professional server
├── models/                # Data structures and types
├── keygenD/              # Key generation and AES encryption utilities
├── go.work               # Go workspace configuration
└── go.work.sum           # Go workspace checksums
```

## 🔑 Key Features

### Proxy Re-Encryption
- Implements cryptographic proxy re-encryption using elliptic curve cryptography (P-256)
- Allows secure delegation of decryption rights without exposing private keys
- Patient can grant and revoke access to healthcare professionals

### Encryption Services
- **Key Generation**: ECDSA key pair generation for patients
- **File Encryption**: Secure file encryption with capsule-based re-encryption
- **Access Control**: Grant and revoke access to encrypted files
- **Re-encryption**: Proxy server transforms encrypted data for authorized users

### Database Integration
- MySQL backend for storing:
  - Patient keys (encrypted)
  - Re-encryption keys
  - User credentials
  - Access permissions

## 🚀 Getting Started

### Prerequisites

- Go 1.22.1 or higher
- MySQL database
- Required Go packages:
  - `github.com/gorilla/mux` - HTTP router
  - `github.com/go-sql-driver/mysql` - MySQL driver
  - `github.com/rs/cors` - CORS middleware
  - Custom `goRecrypt` package for cryptographic operations

### Database Setup

Create three MySQL databases:

```sql
-- Patient database
CREATE DATABASE patiens;

-- Proxy database
CREATE DATABASE proxy;

-- User database
CREATE DATABASE users;
```

### Installation

1. Clone the repository:
```bash
git clone https://github.com/houssemwld1/goserver.git
cd goserver
```

2. Install dependencies:
```bash
go mod download
```

3. Update database credentials in the server files:
   - `serverpatient/server.go`
   - `serverproxy/serverproxy.go`
   - `serveruser/serverprosante.go`

### Running the Servers

Start each server in separate terminals:

```bash
# Terminal 1 - Patient Server
cd serverpatient
go run server.go

# Terminal 2 - Proxy Server
cd serverproxy
go run serverproxy.go

# Terminal 3 - User Server
cd serveruser
go run serverprosante.go
```

## 📡 API Endpoints

### Patient Server (Port 8083)

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/keygen` | POST | Generate encryption keys for a patient |
| `/uploadfile` | POST | Encrypt and upload a file |
| `/giveaccess` | POST | Grant access to a healthcare professional |
| `/downloadfile` | POST | Download and decrypt a file |
| `/removeaccess` | DELETE | Revoke access from a user |

### Proxy Server (Port 8082)

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/saveRKey` | POST | Store re-encryption key |
| `/RenEncCipher` | POST | Re-encrypt cipher for authorized user |
| `/RemoveKey` | POST | Remove re-encryption key |

### User Server (Port 8084)

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/saveUser` | POST | Register a healthcare professional |
| `/DecryptCipher` | POST | Decrypt received cipher |

## 🔐 Security Features

- **AES-256-CBC Encryption**: For key storage in database
- **ECDSA Key Pairs**: Using P-256 elliptic curve
- **Proxy Re-Encryption**: Secure key transformation without exposure
- **Blockchain Address Integration**: Support for blockchain-based identity

## 🛠️ Core Components

### Encryption Logic (`Encryptionlogic/`)
- Key generation and management
- File encryption/decryption
- Access control mechanisms
- Re-encryption key handling

### Key Generation (`keygenD/`)
- AES encryption/decryption utilities
- ECDSA key parsing
- Cryptographic helper functions

### Models (`models/`)
- Data structures for:
  - Capsule data
  - Request/response bodies
  - Cipher texts
  - Re-encryption keys

## 🌐 CORS Configuration

All servers are configured to accept requests from `http://localhost:3000`, making it compatible with React/frontend applications.

## 📝 Example Usage

### 1. Generate Keys for Patient
```bash
curl -X POST http://localhost:8083/keygen \
  -H "Content-Type: application/json" \
  -d '{"blockchain_address": "patient123"}'
```

### 2. Upload Encrypted File
```bash
curl -X POST http://localhost:8083/uploadfile \
  -H "Content-Type: application/json" \
  -d '{
    "blockchain_address": "patient123",
    "file_to_be_encrypted": "<file_data>"
  }'
```

### 3. Grant Access
```bash
curl -X POST http://localhost:8083/giveaccess \
  -H "Content-Type: application/json" \
  -d '{
    "blockchain_address": "patient123",
    "user_address": "doctor456",
    "user_pubkey": "<doctor_public_key>"
  }'
```

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is open source and available under the MIT License.

## 👤 Author

**houssemwld1**
- GitHub: [@houssemwld1](https://github.com/houssemwld1)

## 🔗 Repository

[https://github.com/houssemwld1/goserver](https://github.com/houssemwld1/goserver)

---

**Note**: This is a proof-of-concept implementation. For production use, ensure proper security audits, error handling, and credential management.
