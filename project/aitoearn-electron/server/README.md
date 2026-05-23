# Kaituantuan Server

## Introduction

Kaituantuan Server is a backend project built with NestJS to support aggregated business platform scenarios.

## Environment Configuration

The project uses environment variables for configuration management. Configure variables based on your environment:

1. Copy the example configuration file:
   ```bash
   cp .env.example .env
   ```

2. Update values in the `.env` file with your real configuration.

## Deployment

```bash
pm2 start pm2.json
```
