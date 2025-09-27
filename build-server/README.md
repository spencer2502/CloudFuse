# CloudFuse Build Server

This directory contains a Dockerized build server that automates the process of cloning a Git repository, building a Node.js project, and uploading the build artifacts to an AWS S3 bucket.

## Features

- Clones a specified Git repository into the container
- Installs dependencies and runs the build process (`npm install` and `npm run build`)
- Uploads the contents of the `dist` folder to an S3 bucket (`cloudfuse-outputs`)
- Uses environment variables for configuration and AWS credentials

## File Overview

- `Dockerfile`: Sets up the build environment with Node.js, Git, and required scripts
- `main.sh`: Bash script that clones the repository and starts the Node.js process
- `script.js`: Node.js script that builds the project and uploads build artifacts to S3
- `.env`: Stores AWS credentials (should be kept secure and not committed to public repos)
- `package.json`: Node.js dependencies (not shown here)

## Usage

### 1. Set Environment Variables

You must provide the following environment variables:

- `GIT_REPOSITORY__URL`: The URL of the Git repository to clone
- `PROJECT_ID`: A unique identifier for the build (used as a folder in S3)
- `ACCESSKEYID` and `SECRETACCESSKEY`: AWS credentials (can be set in `.env` or as environment variables)

### 2. Build the Docker Image

```sh
docker build -t cloudfuse-build-server .
```

### 3. Run the Container

```sh
docker run --env GIT_REPOSITORY__URL=<your-repo-url> \
           --env PROJECT_ID=<your-project-id> \
           --env ACCESSKEYID=<your-access-key> \
           --env SECRETACCESSKEY=<your-secret-key> \
           cloudfuse-build-server
```

Alternatively, you can use a `.env` file for AWS credentials and pass the other variables as needed.

### 4. Output

- The build artifacts from the `dist` folder will be uploaded to the S3 bucket `cloudfuse-outputs` under the path `__outputs/<PROJECT_ID>/`.

## Security Note

- **Do not commit your `.env` file or AWS credentials to public repositories.**
- Use IAM roles or environment variables for production deployments.

## Requirements

- Docker
- AWS S3 bucket named `cloudfuse-outputs` in the `ap-south-1` region
- The target repository must have a standard Node.js build process (`npm install` and `npm run build`)

## License

MIT
