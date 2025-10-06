# ---------- Stage 1: Build ----------
FROM node:14-slim AS build

WORKDIR /usr/src/app

# Copy dependency files
COPY package*.json ./

# Install dependencies (dev included)
RUN npm install

# Copy the frontend source
COPY . .

# Run the build step (important for React/Vue/Angular apps)
RUN npm run build


# ---------- Stage 2: Runtime ----------
FROM node:14-alpine AS runtime

WORKDIR /app

# Copy only package files for production install
COPY --from=build /usr/src/app/package*.json ./
RUN npm install --only=production

# Copy build output and any needed files
COPY --from=build /usr/src/app ./

EXPOSE 3000

# Start frontend (npm start from package.json)
CMD ["npm", "start"]
