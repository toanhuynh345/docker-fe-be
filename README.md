# Docker Setup for Demo App

This Docker setup provides containerization for both the backend (NestJS) and frontend (Next.js) applications.

## 🚀 Quick Start

### Production Mode
```bash
cd docker
docker-compose up -d
```

### Development Mode
```bash
cd docker
docker-compose -f docker-compose.dev.yml up -d
```

## 📋 Services

- **Backend (NestJS)**: Runs on port `3001`
- **Frontend (Next.js)**: Runs on port `3000`

## 🛠️ Available Commands

### Production Commands
```bash
# Start all services in production mode
docker-compose up -d

# Stop all services
docker-compose down

# View logs
docker-compose logs -f

# View logs for specific service
docker-compose logs -f backend
docker-compose logs -f frontend

# Rebuild and start
docker-compose up -d --build

# Remove containers and networks
docker-compose down --volumes --remove-orphans
```

### Development Commands
```bash
# Start all services in development mode (with hot reload)
docker-compose -f docker-compose.dev.yml up -d

# Stop development services
docker-compose -f docker-compose.dev.yml down

# View development logs
docker-compose -f docker-compose.dev.yml logs -f

# Rebuild development containers
docker-compose -f docker-compose.dev.yml up -d --build
```

## 🏗️ Architecture

### Backend (NestJS)
- **Base Image**: Node.js 18 Alpine
- **Build Strategy**: Multi-stage build (development, build, production)
- **Port**: 3001
- **Health Check**: Node.js version check
- **Security**: Runs as non-root user

### Frontend (Next.js)
- **Base Image**: Node.js 18 Alpine
- **Build Strategy**: Multi-stage build with standalone output
- **Port**: 3000
- **Health Check**: HTTP request to localhost:3000
- **Security**: Runs as non-root user

## 🔧 Configuration

### Environment Variables

#### Backend
- `NODE_ENV`: Set to `production` or `development`
- `PORT`: Application port (default: 3001)

#### Frontend
- `NODE_ENV`: Set to `production` or `development`
- `NEXT_TELEMETRY_DISABLED`: Disables Next.js telemetry
- `PORT`: Application port (default: 3000)
- `HOSTNAME`: Bind address (default: 0.0.0.0)

### Networks
Both services run on a custom bridge network `demo-app-network` for inter-container communication.

## 🐛 Troubleshooting

### Common Issues

1. **Port conflicts**: Ensure ports 3000 and 3001 are available
2. **Build failures**: Check if all dependencies are properly listed in package.json
3. **Permission issues**: Containers run as non-root users for security

### Useful Commands
```bash
# Check running containers
docker ps

# Inspect a specific container
docker inspect demo-backend

# Execute commands in running container
docker exec -it demo-backend sh

# View container resource usage
docker stats

# Clean up unused Docker resources
docker system prune -a
```

## 📁 File Structure
```
docker/
├── docker-compose.yml          # Production configuration
├── docker-compose.dev.yml      # Development configuration
└── README.md                   # This file

backend/
├── Dockerfile                  # Backend Docker configuration
├── .dockerignore              # Files to exclude from build
└── ... (NestJS application files)

frontend/
├── Dockerfile                  # Frontend Docker configuration
├── .dockerignore              # Files to exclude from build
└── ... (Next.js application files)
```

## 🔄 Development Workflow

1. **First time setup**:
   ```bash
   cd docker
   docker-compose -f docker-compose.dev.yml up -d --build
   ```

2. **Daily development**:
   ```bash
   cd docker
   docker-compose -f docker-compose.dev.yml up -d
   ```

3. **Code changes**: Files are mounted as volumes, so changes reflect immediately with hot reload

4. **Testing production build**:
   ```bash
   cd docker
   docker-compose up -d --build
   ```

## 🚀 Deployment

For production deployment:
1. Use the main `docker-compose.yml` file
2. Configure environment variables as needed
3. Set up reverse proxy (nginx) if required
4. Configure SSL certificates
5. Set up monitoring and logging

## 📞 Support

If you encounter any issues:
1. Check the logs: `docker-compose logs -f`
2. Verify all services are running: `docker ps`
3. Ensure ports are not in use by other applications
4. Try rebuilding: `docker-compose up -d --build` 