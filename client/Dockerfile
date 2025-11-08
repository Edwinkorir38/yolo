# Stage 1: Build
FROM node:14-alpine AS build

# Set working directory
WORKDIR /app

# Copy dependency files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy source code
COPY . .

# Accept REACT_APP_API_URL as a build argument and set it as environment variable
ARG REACT_APP_API_URL
ENV REACT_APP_API_URL=$REACT_APP_API_URL

# Build the React app (this will bake REACT_APP_API_URL into the build)
RUN npm run build

# Stage 2: Runtime
FROM nginx:alpine AS runtime

# Copy the build output to Nginx
COPY --from=build /app/build /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
