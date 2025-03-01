# Mealie Server on Fly.io

This repository contains the configuration for running a [Mealie](https://hay-kot.github.io/mealie/) recipe manager instance on Fly.io.

## Deployment

Here are some brief instructions on how to deploy your own instance:

1. Install the [Fly.io CLI](https://fly.io/docs/hands-on/install-flyctl/)
2. Fork and clone this repository
3. (Optional) Set up your SMTP password:
   ```bash
   fly secrets set SMTP_PASSWORD="your-password-here"
   ```
4. Deploy the application:
   ```bash
   fly deploy
   ```

Afterwards, dive into [`fly.toml`](fly.toml) to customize the configuration to your liking.

## Configuration

The server is configured with:
- Auto-scaling enabled (0-1 instances)
- 512MB RAM
- 1 shared CPU
- TLS encryption
- MailerSend SMTP for email notifications
- European timezone (Berlin)
- Signup disabled
- Base URL: https://recipes.aknapen.nl

## Storage

The application uses a persistent volume mount for data storage:
- Mount point: `/app/data`
- Auto-extends when 80% full
- Increases in 1GB increments
- Maximum size: 3GB

## CI/CD

The application automatically deploys to Fly.io when changes are pushed to the main branch. To set this up:

1. Create a Fly.io API token
  ```bash
  fly tokens create deploy --name "GitHub Actions - deploy"
  ```
2. Add the token as a GitHub secret named `FLY_API_TOKEN`
  ```bash
  gh secret set FLY_API_TOKEN
  ```

> 💡 **Tip:** You can combine the token creation and secret setting in one command:
> ```bash
> gh secret set FLY_API_TOKEN -b $(fly tokens create deploy --name "GitHub Actions - deploy")
> ```
