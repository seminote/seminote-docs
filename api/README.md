# 🔌 Seminote API Documentation

> **Comprehensive API specifications for the Seminote Piano Learning Platform**

## 📋 Overview

The Seminote platform exposes multiple APIs across different services to support the speed-adaptive hybrid architecture. This documentation covers all service interfaces, protocols, and integration patterns.

## 🏗️ API Architecture

### Service Breakdown

| Service | Technology | Port | Purpose |
|---------|------------|------|---------|
| **Backend API** | Java/Spring Boot | 8080 | Core business logic |
| **Real-time API** | Node.js/Express | 3000 | WebRTC & streaming |
| **ML API** | Python/FastAPI | 8000 | Machine learning services |
| **Edge API** | Go/Gin | 9000 | Edge computing services |

### API Gateway
- **Technology**: Spring Cloud Gateway
- **Features**: Rate limiting, authentication, load balancing
- **External Gateway**: AWS API Gateway for public endpoints

## 🔐 Authentication & Authorization

### JWT Token Flow
```
Client → Auth Service → JWT Token → API Gateway → Service
```

### Authentication Methods
- **OAuth2**: Google, Apple, Facebook integration
- **JWT**: Stateless token-based authentication
- **API Keys**: Service-to-service communication
- **Biometric**: iOS Touch ID/Face ID integration

### Authorization Levels
- **Public**: Unauthenticated access (limited)
- **User**: Authenticated user access
- **Premium**: Subscription-based features
- **Admin**: Administrative operations
- **Service**: Internal service communication

## 📊 API Specifications

### Backend API (Spring Boot)

#### Base URL
```
Production: https://api.seminote.com
Staging: https://staging-api.seminote.com
Local: http://localhost:8080
```

#### Core Endpoints

**User Management**
```http
GET    /api/v1/users/profile
PUT    /api/v1/users/profile
POST   /api/v1/users/register
POST   /api/v1/users/login
DELETE /api/v1/users/account
```

**Content Management**
```http
GET    /api/v1/content/lessons
GET    /api/v1/content/lessons/{id}
GET    /api/v1/content/sheet-music
POST   /api/v1/content/progress
```

**Analytics**
```http
POST   /api/v1/analytics/session
GET    /api/v1/analytics/progress
GET    /api/v1/analytics/insights
POST   /api/v1/analytics/feedback
```

### Real-time API (Node.js)

#### Base URL
```
Production: wss://realtime.seminote.com
Staging: wss://staging-realtime.seminote.com
Local: ws://localhost:3000
```

#### WebRTC Endpoints
```http
POST   /api/v1/webrtc/session/create
POST   /api/v1/webrtc/session/join
DELETE /api/v1/webrtc/session/{id}
GET    /api/v1/webrtc/session/{id}/status
```

#### Socket.io Events
```javascript
// Client to Server
'audio-stream-start'
'audio-stream-data'
'audio-stream-end'
'tempo-change'

// Server to Client
'feedback-basic'
'feedback-detailed'
'session-analysis'
'error'
```

### ML API (Python/FastAPI)

#### Base URL
```
Production: https://ml.seminote.com
Edge: https://edge-{region}.seminote.com
Local: http://localhost:8000
```

#### ML Endpoints
```http
POST   /api/v1/ml/analyze/audio
POST   /api/v1/ml/analyze/session
GET    /api/v1/ml/models/status
POST   /api/v1/ml/feedback/generate
```

### Edge API (Go)

#### Base URL
```
Edge Nodes: https://edge-{region}.seminote.com:9000
Local: http://localhost:9000
```

#### Edge Endpoints
```http
POST   /api/v1/edge/process/audio
GET    /api/v1/edge/health
GET    /api/v1/edge/metrics
POST   /api/v1/edge/feedback/realtime
```

## 📡 Communication Protocols

### HTTP/REST
- **Standard**: RESTful APIs with JSON payloads
- **Versioning**: URL-based versioning (/api/v1/)
- **Status Codes**: Standard HTTP status codes
- **Rate Limiting**: Token bucket algorithm

### WebRTC
- **Audio Streaming**: Real-time audio transmission
- **Encryption**: SRTP for secure audio streams
- **Codecs**: Opus for audio compression
- **Fallback**: WebSocket streaming if WebRTC fails

### WebSocket
- **Real-time Updates**: Progress, feedback, notifications
- **Heartbeat**: Keep-alive mechanism
- **Reconnection**: Automatic reconnection with exponential backoff
- **Authentication**: JWT token in connection headers

## 🔄 Data Models

### Common Response Format
```json
{
  "success": true,
  "data": {},
  "message": "Operation successful",
  "timestamp": "2025-05-25T21:00:00Z",
  "requestId": "uuid-v4"
}
```

### Error Response Format
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input parameters",
    "details": {}
  },
  "timestamp": "2025-05-25T21:00:00Z",
  "requestId": "uuid-v4"
}
```

### Audio Analysis Request
```json
{
  "audioData": "base64-encoded-audio",
  "format": "wav",
  "sampleRate": 44100,
  "channels": 1,
  "processingMode": "edge|local|hybrid",
  "sessionId": "uuid-v4"
}
```

### Feedback Response
```json
{
  "feedback": {
    "notes": [
      {
        "note": "C4",
        "accuracy": 0.95,
        "timing": 0.98,
        "suggestions": ["Slightly sharp"]
      }
    ],
    "rhythm": {
      "accuracy": 0.92,
      "tempo": 120,
      "suggestions": ["More consistent timing"]
    },
    "overall": {
      "score": 0.94,
      "level": "intermediate",
      "nextSteps": ["Practice scales"]
    }
  }
}
```

## 📈 Rate Limiting

### Limits by Endpoint Type
- **Authentication**: 10 requests/minute
- **Content Retrieval**: 100 requests/minute
- **Audio Analysis**: 60 requests/minute
- **Real-time Streaming**: No limit (connection-based)

### Rate Limiting Headers
```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1640995200
```

## 🧪 Testing & Development

### API Testing
- **Postman Collection**: Available in `/api/postman/`
- **OpenAPI Spec**: Swagger documentation at `/docs`
- **Test Environment**: Staging APIs for integration testing

### SDK & Libraries
- **iOS SDK**: Swift package for iOS integration
- **JavaScript SDK**: NPM package for web integration
- **Python SDK**: PyPI package for ML integration

## 📁 Detailed Documentation

- [`backend-api.md`](./backend-api.md) - Complete Spring Boot API reference
- [`realtime-api.md`](./realtime-api.md) - WebRTC and Socket.io documentation
- [`ml-api.md`](./ml-api.md) - Machine learning service APIs
- [`edge-api.md`](./edge-api.md) - Edge computing service APIs

---

**🔌 Connecting all components of the Seminote ecosystem through robust APIs**
