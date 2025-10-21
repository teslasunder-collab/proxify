### 🐳 Option 1: Deploy with Docker (Recommended)

We provide three convenient Docker deployment methods.

#### 1. Pull from Docker Hub (Simplest)

This is the fastest and most recommended way to deploy in production.

```bash
# 1. Pull the latest image from Docker Hub
docker pull poixeai/proxify:latest

# 2. Run the container and mount configuration files
docker run -d \
  --name proxify \
  -p 7777:7777 \
  -v $(pwd)/routes.json:/app/routes.json \
  -v $(pwd)/.env:/app/.env \
  --restart=always \
  poixeai/proxify:latest
```

#### 2. Use Docker Compose (Recommended)

Manage your service declaratively via a `docker-compose.yml` file for better maintainability.

1. **Ensure the `docker-compose.yml` file exists in your current directory.**

2. **Start the service:**

   ```bash
   # Start the service
   docker-compose up -d

   # Check service status
   docker-compose ps
   ```

#### 3. Build from Dockerfile

If you want to build your own image based on the latest source code.

```bash
# 1. Clone the repository
git clone https://github.com/poixeai/proxify.git
cd proxify

# 2. Build your own image (for example, name it my-proxify)
docker build -t poixeai/proxify:latest .

# 3. Run the image you just built
docker run -d \
  --name proxify \
  -p 7777:7777 \
  -v $(pwd)/routes.json:/app/routes.json \
  -v $(pwd)/.env:/app/.env \
  --restart=always \
  poixeai/proxify:latest
```

---

### 🛠️ Option 2: Manual Build and Run

For development environments or when Docker is not preferred.

**Requirements:**

* Go (version 1.20+)
* Node.js (version 18+)
* pnpm

#### 1. Use the Build Script (Recommended)

We provide a `build.sh` script to simplify the compilation process.

```bash
# 1. Clone the repository and enter the directory
git clone https://github.com/poixeai/proxify.git
cd proxify

# 2. Grant execution permission to the script
chmod +x build.sh

# 3. Run the build script
./build.sh

# 4. Run the compiled binary
./bin/proxify
```

#### 2. Fully Manual Build

If you prefer to understand the full build process.

```bash
# 1. Clone the repository and enter the directory
git clone https://github.com/poixeai/proxify.git
cd proxify

# 2. Build frontend static assets
cd web
pnpm install
pnpm build
cd ..

# 3. Build backend Go application
go mod tidy
go build -o ./bin/proxify .

# 4. Run the binary
./bin/proxify
```