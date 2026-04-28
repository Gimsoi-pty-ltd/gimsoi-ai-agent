# Stage 1: Builder - Install dependencies and generate Prisma Client
# This stage uses a Node.js image with development tools for building.
FROM node:20-slim as builder

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json first to leverage Docker cache.
# If these files don't change, Docker can reuse this layer, speeding up builds.
COPY package.json package-lock.json ./
# Install ALL dependencies (including devDependencies like Prisma CLI)
# as Prisma generate needs them during this build stage.
RUN npm install

# Copy the rest of your application source code
COPY . .
# Ensure the prisma schema is copied to the correct location within the container
COPY prisma/ ./prisma/


# Generate Prisma Client - crucial for the runtime environment.
# This command uses the Prisma CLI installed in the previous step to generate
# the client based on your schema.prisma, making it available for your app.
RUN npx prisma generate

# If your project uses TypeScript and compiles to a 'dist' folder, uncomment these lines:
# RUN npm install -g typescript
# RUN tsc --outDir dist # Compiles TypeScript from src to JavaScript in a 'dist' folder


# -----------------------------------------------------------------------------
# Stage 2: Runner - A smaller, production-ready image for deployment
# This stage uses a lighter Node.js image and copies only essential runtime files.
FROM node:20-slim

# Set the working directory inside the container
WORKDIR /app

# Copy only necessary files from the builder stage to reduce final image size.
# node_modules are copied for runtime dependencies (dev dependencies are omitted implicitly).
COPY --from=builder /app/node_modules ./node_modules
# Prisma client and related files are needed at runtime.
COPY --from=builder /app/prisma ./prisma
# package.json (can be useful for introspection, but mostly for consistency).
COPY --from=builder /app/package.json ./package.json

# Copy your application's source code (JavaScript files).
# IMPORTANT: If you compiled TypeScript to a 'dist' folder, ADJUST this line to:
# COPY --from=builder /app/dist ./dist
# Based on your `package.json`'s `main: "src/index.js"`, we assume source JS is in src/.
COPY --from=builder /app/src ./src

# Expose the port your Express app listens on.
# Cloud Run expects your application to listen on the port specified by the PORT environment variable,
# which defaults to 8080. Our index.js is configured for this.
EXPOSE 8080

# Command to start your application when the container launches.
# IMPORTANT: This command uses 'src/index.js' as per your package.json's `main` and `start` scripts.
CMD ["node", "src/index.js"]
