FROM node:alpine
RUN apk add --no-cache nodejs npm
RUN npm install -g pm2

# Set the working directory
WORKDIR /api

# Create a non-root user to run the application
RUN addgroup -S appuser && adduser -S appuser -G appuser

# Change ownership of the working directory
RUN chown -R appuser:appuser /api

# Switch to the non-root user
USER appuser

# Copy package.json and package-lock.json
COPY --chown=appuser:appuser package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY --chown=appuser:appuser . .

# Expose the application port
EXPOSE 5050

# Start the application
CMD ["pm2-runtime", "start", "server.js", "--name", "ostad-backend"]
