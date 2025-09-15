FROM node:alpine AS builder

# Install build dependencies
RUN apk add --no-cache nodejs npm

# Set the working directory
WORKDIR /app

# Create a non-root user to run the application
RUN addgroup -S appuser && adduser -S appuser -G appuser

RUN chown -R appuser:appuser /app

# Switch to the non-root user
USER appuser

# Set environment variables
ARG VITE_API_BASE_URL_ARG
ENV VITE_API_BASE_URL=${VITE_API_BASE_URL_ARG}


# Install application dependencies
COPY --chown=appuser:appuser package*.json ./
RUN npm install

# Copy the rest of the application code
COPY --chown=appuser:appuser . .

# Build the application
RUN npm run build

# Use a minimal base image for the final stage
FROM nginx:alpine AS production

# Copy the custom Nginx configuration file
# This file will tell Nginx to listen on port 5173
COPY nginx.conf /etc/nginx/conf.d/default.conf

COPY --from=builder /app/dist /usr/share/nginx/html

# Expose the application port
EXPOSE 5173

# run Nginx
CMD ["nginx", "-g", "daemon off;"]
