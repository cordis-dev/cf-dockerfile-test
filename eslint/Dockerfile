# Stage 1: Builder
FROM node:alpine AS builder

# Install packages
RUN npm install -g --force @angular-eslint/eslint-plugin-template@17.3.0 @angular-eslint/eslint-plugin@17.3.0 @angular-eslint/template-parser@17.3.0 @angular/compiler@17.3.5 @antfu/eslint-config-basic@0.43.1 @antfu/eslint-config-ts@0.43.1 @antfu/eslint-config-vue@0.43.1 @antfu/eslint-config@v0.43.1

# Copy modified files
COPY cf-node-package-mods-v2 /usr/local/lib/node_modules

# Stage 2: Final image
FROM node:alpine

# Copy the global npm packages and binaries from the builder stage
COPY --from=builder /usr/local /usr/local

# Set the working directory
WORKDIR /app

# Create an entrypoint script with explicit /bin/sh path
COPY <<'EOF' /entrypoint.sh
#!/bin/sh
ln -sf /usr/local/lib/node_modules /app/node_modules
exec "$@"
EOF

RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/bin/sh", "/entrypoint.sh"]
CMD ["eslint", "."]
