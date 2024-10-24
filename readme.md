# Discovery Server - Real-time Chat Application

## Overview
This Discovery Server is a crucial component of our Real-time Chat Application microservices architecture. It serves as a service registry and discovery server using Netflix Eureka, enabling dynamic service-to-service communication across the application.

The Real-time Chat Application allows users to:
- Register and authenticate
- Send private messages to friends
- Create and manage group chats
- Receive real-time notifications
- Manage friend requests and relationships

## Architecture Overview

### System Architecture Diagram
```mermaid
graph TD
    Client[Client Applications] -->|Access through| Gateway[API Gateway]
    Gateway -->|Discovers Services| Discovery[Discovery Server]
    Auth[Auth Service] -->|Registers| Discovery
    User[User Service] -->|Registers| Discovery
    Chat[Chat Service] -->|Registers| Discovery
    Group[Group Chat Service] -->|Registers| Discovery
    Notification[Notification Service] -->|Registers| Discovery
    
    style Discovery fill:#f96,stroke:#333,stroke-width:4px
```

### Discovery Server Role
The Discovery Server acts as a "phone book" for all microservices in our chat application:
1. **Service Registration**: Each microservice registers itself with the Discovery Server
2. **Service Discovery**: Services can locate other services without hardcoding URLs
3. **Load Balancing**: Supports multiple instances of services
4. **Health Monitoring**: Tracks the health status of registered services

## Technical Details

### Key Features
- Service registration and discovery
- Health monitoring
- Dashboard UI for system status
- Basic security with authentication
- Load balancing support
- High availability configuration options

### Built With
- Java 17
- Spring Boot 3.2.0
- Spring Cloud Netflix Eureka Server
- Spring Security
- Maven

### Configuration Properties
```yaml
server:
  port: 8761  # Default Eureka port

spring:
  application:
    name: discovery-server
  security:
    user:
      name: ${EUREKA_USERNAME:eureka}
      password: ${EUREKA_PASSWORD:password}

eureka:
  instance:
    hostname: localhost
  client:
    register-with-eureka: false
    fetch-registry: false
```

## Getting Started

### Prerequisites
- Java 17 or higher
- Maven 3.6 or higher
- Docker (optional)

### Installation

1. Clone the repository
```bash
git clone https://github.com/your-org/discovery-server.git
cd discovery-server
```

2. Build the project
```bash
mvn clean package
```

3. Run the application
```bash
java -jar target/discovery-server-1.0-SNAPSHOT.jar
```

### Docker Support

Build the Docker image:
```bash
docker build -t chat-app/discovery-server .
```

Run the container:
```bash
docker run -p 8761:8761 chat-app/discovery-server
```

### Verification
1. Access the Eureka Dashboard: `http://localhost:8761`
2. Login with credentials:
    - Username: `eureka`
    - Password: `password`

## Service Integration

Other services can register with Eureka by adding these properties:
```yaml
eureka:
  client:
    serviceUrl:
      defaultZone: http://eureka:password@localhost:8761/eureka/
  instance:
    preferIpAddress: true
```

## Security

The Discovery Server implements basic security measures:
- Basic authentication for dashboard access
- CSRF protection
- Secure communication options
- Role-based access control

## Monitoring

The service provides several endpoints for monitoring:
- `/actuator/health` - Health check endpoint
- `/eureka/apps` - Registered applications
- `/eureka/status` - Server status

## Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `EUREKA_USERNAME` | Dashboard username | eureka |
| `EUREKA_PASSWORD` | Dashboard password | password |
| `SERVER_PORT` | Server port | 8761 |

## Development

### Project Structure
```
discovery-server/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── chatapp/
│   │   │           └── discovery/
│   │   │               ├── DiscoveryServerApplication.java
│   │   │               └── config/
│   │   │                   └── SecurityConfig.java
│   │   └── resources/
│   │       └── application.yml
├── Dockerfile
└── pom.xml
```

### Build and Test
```bash
# Run tests
mvn test

# Build
mvn clean package

# Build with Docker
docker build -t chat-app/discovery-server .
```

## Production Considerations

### High Availability
For production deployment, consider:
- Running multiple instances
- Configuring peer awareness
- Setting up proper security measures
- Monitoring and alerting
- Regular backup of registry

### Security Hardening
- Use strong passwords
- Enable HTTPS
- Configure proper network security
- Implement rate limiting
- Regular security updates

## Contributing
1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

