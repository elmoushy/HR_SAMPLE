# Frappe HRMS Docker Setup

This guide provides complete instructions to set up and run the Frappe HRMS (Human Resource Management System) using Docker.

## Prerequisites

Before starting, ensure you have the following installed on your system:

- **Docker** - [Download Docker](https://docs.docker.com/get-docker/)
- **Docker Compose** - Usually included with Docker Desktop
- **Git** - [Download Git](https://git-scm.com/downloads)

## Quick Start

Follow these steps to get the HRMS application running:

### 1. Clone the Repository

```bash
git clone https://github.com/elmoushy/HR_SAMPLE.git
cd HR_SAMPLE/docker
```

### 2. Start the Application

```bash
docker-compose up -d
```

This command will:
- Download all required Docker images (MariaDB, Redis, Frappe)
- Set up the database
- Install Frappe, ERPNext, and HRMS applications
- Configure the system
- Start all services in the background

**Note:** The first startup takes 15-20 minutes as it:
- Initializes the Frappe bench
- Installs Python and Node.js dependencies
- Builds frontend assets
- Creates the database and site
- Installs all apps

### 3. Monitor Setup Progress

To watch the setup progress, run:

```bash
docker logs docker-frappe-1 -f
```

Press `Ctrl+C` to exit the logs view (the container will continue running).

### 4. Access the Application

Once setup is complete, open your browser and navigate to:

```
http://localhost:8000
```

**Default Login Credentials:**
- Username: `Administrator`
- Password: `admin`

## Container Management

### View Running Containers

```bash
docker ps
```

### Stop the Application

```bash
docker-compose down
```

### Stop and Remove All Data (Fresh Start)

```bash
docker-compose down -v
```

**Warning:** The `-v` flag removes all volumes, including the database. Use this only if you want to start completely fresh.

### View Logs

View logs for all containers:
```bash
docker-compose logs -f
```

View logs for a specific container:
```bash
docker logs docker-frappe-1 -f    # Frappe application
docker logs docker-mariadb-1 -f   # Database
docker logs docker-redis-1 -f     # Redis cache
```

### Restart Containers

```bash
docker-compose restart
```

## System Architecture

The application runs with three main containers:

1. **Frappe Container** (docker-frappe-1)
   - Runs the Frappe web application
   - Includes ERPNext and HRMS apps
   - Accessible on port 8000

2. **MariaDB Container** (docker-mariadb-1)
   - Database server
   - Stores all application data
   - Accessible on port 3306

3. **Redis Container** (docker-redis-1)
   - Cache and queue management
   - Background job processing
   - Accessible on port 6379

## Troubleshooting

### Container Exits Immediately

If the frappe container exits repeatedly, check the logs:

```bash
docker logs docker-frappe-1
```

Common issues:
- Database not ready: Wait a few seconds and the container will retry
- Port conflicts: Ensure ports 8000, 3306, and 6379 are not in use

### Cannot Access the Application

1. Verify containers are running:
   ```bash
   docker ps
   ```

2. Check if all three containers are listed (frappe, mariadb, redis)

3. Try accessing: `http://127.0.0.1:8000` instead of `localhost:8000`

### Reset Everything

To start fresh:

```bash
docker-compose down -v
docker-compose up -d
```

## Configuration

### Database Configuration

The database is configured with:
- Root password: `123`
- Database name: `hrms.localhost`

### Frappe Configuration

The Frappe site is configured with:
- Site name: `hrms.localhost`
- Developer mode: Enabled
- Scheduler: Enabled

## Advanced Usage

### Access Frappe Bench Console

To run bench commands inside the container:

```bash
docker exec -it docker-frappe-1 bash
cd frappe-bench
bench --help
```

### Create Additional Sites

Inside the container:

```bash
bench new-site mysite.localhost \
  --mariadb-root-password 123 \
  --admin-password admin \
  --no-mariadb-socket
```

### Install Additional Apps

```bash
bench get-app app-name
bench --site mysite.localhost install-app app-name
```

## Production Deployment

**Important:** This Docker setup is configured for development purposes. For production:

1. Disable developer mode
2. Use strong passwords
3. Set up SSL/HTTPS
4. Configure backup strategies
5. Review security settings
6. Use environment variables for sensitive data

## System Requirements

**Minimum Requirements:**
- 4GB RAM
- 2 CPU cores
- 20GB free disk space

**Recommended:**
- 8GB RAM or more
- 4 CPU cores
- 50GB free disk space

## Support

For issues related to:
- **Docker setup**: Check this README or open an issue
- **Frappe/ERPNext**: Visit [Frappe Forum](https://discuss.frappe.io/)
- **HRMS**: Visit [Frappe HR Documentation](https://frappeframework.com/)

## License

This project uses:
- Frappe Framework (MIT License)
- ERPNext (GNU General Public License)
- Frappe HR (GNU Affero General Public License)

Refer to individual applications for specific licensing terms.

## Features

Frappe HRMS includes:

- **Employee Management**: Complete employee records and documentation
- **Leave Management**: Leave applications, approvals, and balance tracking
- **Attendance**: Check-in/check-out, shift management, attendance tracking
- **Payroll**: Salary structures, salary slips, tax calculations
- **Expense Claims**: Employee expense submissions and approvals
- **Performance**: Appraisals, KPIs, and goal tracking
- **Recruitment**: Job postings, applications, interview scheduling
- **Training**: Training programs and employee skill development
- **Reports**: Comprehensive HR analytics and reporting

## Next Steps

After successful setup:

1. Complete the Setup Wizard on first login
2. Configure your company details
3. Set up employee records
4. Configure leave types and policies
5. Set up salary structures
6. Explore the various HR modules

Enjoy using Frappe HRMS! 🎉
