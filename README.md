# Github Actions Based Monitoring
GitHub Actions workflow that monitors external services availability.

## Monitored Services
The strategy here to save money is to include all of your monitored services in the same github action and then report on them individually. At the time of this writing there is a publicly accessible SFTP server at test.rebex.net:22. Also at the time of this writing my personal site https://www.nestorurquiza.com is available. The github actions workflow file here in this project point to them.
* SFTP (`test.rebex.net:22`)
* Portal (`www.nestorurquiza.com:443`)

## Features
* Runs every 5 minutes
* Tests SFTP connection and HTTPS service availability but others can be added
* Creates GitHub issues when services are down
* Sends email notifications on failure via SendGrid
* Comprehensive failure reporting (identifies which specific services failed)
* Supports manual trigger for on-demand testing

## Setup
1. Clone repository
2. Workflow file is in `.github/workflows/network_connectivity_tests.yml`
3. Configure SendGrid Integration:
   * Set up a SendGrid account and obtain an API key
   * Add the API key to repository secrets as `SENDGRID_API_KEY`
   * Verify sender email address in SendGrid

## Tests Performed
In the included workflow file as mentioned I am including just two services but you should add as many as needed following the exact same patterns
* SFTP: Tests TCP connection to port 22
* HTTPS: Verifies HTTPS connection and checks for expected content

## Notification System
* GitHub Issues: Created automatically when services fail
* Email Notifications: 
  * Sent via SendGrid
  * Include specific failed services in subject and body
  * Timestamp of failure included
  * Multiple recipients supported

## Cost analysis
* Uses GitHub Actions minutes (free tier: 2,000 minutes/month)
* ~8,928 minutes/month with 5-minute intervals
* Additional minutes: $0.008/minute
* SendGrid free tier includes 100 emails/day

## Manual Testing
Use "Run workflow" button in Actions tab for on-demand checks.

## Troubleshooting
* SFTP failures are detected when:
  * Connection timeout occurs
  * Connection is refused
  * DNS resolution fails
* HTTPS failures are detected when:
  * Connection timeout occurs
  * HTTP response doesn't contain expected content
  * SSL/TLS errors occur
