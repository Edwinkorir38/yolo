# ---------- Stage 1: Build ----------
FROM node:14 AS build

WORKDIR /usr/src/app

# Copy dependency files
COPY package*.json ./

# Install dependencies (dev included for build)
RUN npm install

# Copy source code
COPY . .

# If you had a build step (e.g. TS), you’d run it here:
# RUN npm run build


# ---------- Stage 2: Runtime ----------
FROM node:14-alpine AS runtime

WORKDIR /app

# Copy only package files for production install
COPY --from=build /usr/src/app/package*.json ./
RUN npm install --only=production

# Copy built app (or plain code since this is JS backend)
COPY --from=build /usr/src/app ./

EXPOSE 5000

# Start the app using package.json script
CMD ["npm", "start"]

