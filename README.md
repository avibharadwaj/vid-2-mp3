# Microservice Architecture and System Design with Python & Kubernetes

This repository contains the code for a microservice-based application that converts video files to MP3s. This project was built following the "Microservice Architecture and System Design with Python & Kubernetes – Full Course" from freeCodeCamp.

## Overview

The application is designed using a microservices architecture, where different functionalities are separated into independent services that communicate with each other. The core functionality involves:

1.  **User Upload:** A user uploads a video file.
2.  **Storage:** The video is stored in MongoDB.
3.  **Message Queue:** A message is placed on a RabbitMQ queue, signaling the need for conversion.
4.  **Conversion:** A dedicated service converts the video to MP3.
5.  **Notification:** The user is notified via email when the MP3 is ready.
6.  **Download:** The user can download the MP3 using a unique ID and JWT.

## Technologies Used

*   **Python:** The primary programming language for all services.
*   **RabbitMQ:** A message broker for asynchronous communication between services.
*   **MongoDB:** A NoSQL database for storing video and MP3 files.
*   **Docker:** For containerizing the services.
*   **Kubernetes:** For orchestrating and managing the containerized services.
*   **MySQL:** For user authentication data.
*   **Flask:** A micro web framework for creating the API endpoints.
*   **JWT (JSON Web Tokens):** For secure authentication and authorization.

## Services

The application is composed of the following microservices:

*   **Auth Service:** Handles user authentication and generates JWTs.
*   **API Gateway:** Serves as the entry point for the application, handling user requests and routing them to the appropriate services.
*   **Video to MP3 Converter Service:** Consumes messages from the RabbitMQ queue, converts videos to MP3s, and stores them in MongoDB.
*   **Notification Service:** Consumes messages from the RabbitMQ queue and sends email notifications to users.

## Setup

To run this project, you'll need to have the following installed:

*   **Docker:** For containerization.
*   **Kubernetes CLI (`kubectl`):** For interacting with Kubernetes clusters.
*   **MiniKube:** For running a local Kubernetes cluster.
*   **Python 3.x:** For running the services.
*   **MySQL:** For the auth service database.
*   **RabbitMQ:** For the message queue.
*   **MongoDB:** For storing video and MP3 files.

### Installation Steps

1.  **Clone the repository:**
    ```bash
    git clone [repository URL]
    cd [repository directory]
    ```
2.  **Install dependencies:**
    *   Navigate to each service directory (`auth`, `gateway`, `converter`, `notification`) and create a virtual environment:
        ```bash
        python3 -m venv venv
        source venv/bin/activate  # On Linux/macOS
        venv\Scripts\activate  # On Windows
        pip install -r requirements.txt
        ```
3.  **Configure Environment Variables:**
    *   Set up environment variables for each service as needed. This includes database credentials, RabbitMQ connection details, and Gmail credentials for the notification service.
4.  **Build Docker Images:**
    *   Navigate to each service directory and build the Docker images:
        ```bash
        docker build -t [your-dockerhub-username]/[service-name]:latest .
        ```
5.  **Push Docker Images:**
    *   Push the Docker images to your Docker Hub account:
        ```bash
        docker push [your-dockerhub-username]/[service-name]:latest
        ```
6.  **Start MiniKube:**
    ```bash
    minikube start
    ```
7.  **Enable Ingress Add-on:**
    ```bash
    minikube addons enable ingress
    ```
8.  **Run MiniKube Tunnel:**
    ```bash
    minikube tunnel
    ```
    *   Keep this terminal running.
9.  **Configure `/etc/hosts`:**
    *   Add the following lines to your `/etc/hosts` file:
        ```
        127.0.0.1 mp3converter.com
        127.0.0.1 rabbitmqmanager.com
        ```
10. **Apply Kubernetes Manifests:**
    *   Navigate to the `manifest` directory and apply the Kubernetes configurations:
        ```bash
        kubectl apply -f .
        ```

## Usage

1.  **Login:** Send a POST request to `http://mp3converter.com/login` with basic authentication credentials to obtain a JWT.
2.  **Upload:** Send a POST request to `http://mp3converter.com/upload` with the video file and the JWT in the authorization header.
3.  **Download:** After receiving the email notification, send a GET request to `http://mp3converter.com/download?fid=[file_id]` with the JWT in the authorization header.

## Key Concepts

*   **Microservices Architecture:** Breaking down an application into small, independent services.
*   **Asynchronous Communication:** Using message queues (RabbitMQ) for non-blocking communication between services.
*   **Synchronous Communication:** Direct communication between services, such as the API Gateway and Auth service.
*   **JWT (JSON Web Tokens):** For secure authentication and authorization.
*   **Kubernetes:** For container orchestration and management.
*   **Docker:** For containerization.
*   **GridFS:** For storing large files in MongoDB.
*   **Competing Consumers Pattern:** For scaling message processing.
*   **Strong vs. Eventual Consistency:** Understanding data consistency in distributed systems.

## Contributing

Feel free to fork this repository and submit pull requests with improvements or bug fixes.
