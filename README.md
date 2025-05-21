# 🌸 Hybrid Anime Recommender 🌸

An anime recommendation application based on neural networks, built with a focus on personalization, scalability and automation in deployment.

## 📌 Description

This project consists of an intelligent system that suggests anime to users based on their tastes. The solution was developed using a **neural network trained with user preference data**, and made available via a modern web interface.

The complete pipeline involves:

- ✅ Modeling with Machine Learning (neural network)
- 🐳 Containerization with Docker
- ⚙️ Automated Deployment with Jenkins
- ☸️ Orchestration with Kubernetes (Google Cloud Autopilot)
- 💻 Interactive web interface

## 📷Demo

![App Screenshot](./screenshot.png)

## 🚀 Features

- Personalized anime recommendation
- Stylized web interface with user ID input
- Automatic deployment with CI/CD
- Scalable Application with Kubernetes Autopilot

## 📂 Project Structure

```
anime-recommender/
│
├── notebooks/
│   └── anime.ipynb            # Neural network training
│
├── app/
│   ├── main.py                # Backend code (Flask/FastAPI etc.)
│   └── static/                # HTML/CSS/JS frontend
│
├── Dockerfile
├── jenkinsfile                # CI/CD pipeline
├── k8s-deployment.yaml        # Kubernetes Manifesto
├── requirements.txt
└── README.md
```

## 📦 Requirements

- Python 3.8+
- Docker
- Kubernetes (kubectl + cluster configured)
- Jenkins (optional for CI/CD)

## ⚙️ Local Installation and Execution

1. **Clone the repository:**
   ```bash
   git clone https://github.com/seu-usuario/anime-recommender.git
   cd anime-recommender
   ```

2. **Create a virtual environment and install the dependencies:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # ou .\venv\Scripts\activate no Windows
   pip install -r requirements.txt
   ```

3. **Run the application:**
   ```bash
   python app/main.py
   ```

4. Access via browser: `http://localhost:5000`

## 🐳 Deploy with Docker

```bash
docker build -t anime-recommender .
docker run -p 5000:5000 anime-recommender
```

## ☸️ Deploy with Kubernetes

```bash
kubectl apply -f k8s-deployment.yaml
```

Access the service as per the external URL of your GKE cluster (or configure an Ingress).

## 🔁 Jenkins Pipeline

The project includes a `Jenkinsfile` for CI/CD, which:

- Build the Docker image
- Pushes to a container registry
- Automatically apply Kubernetes configuration

## 📊 Model Training

The model was trained using a dataset of user ratings of anime. The neural network was implemented with Keras/TensorFlow in the `anime.ipynb` notebook.

## 🧠 Technologies Used

- Python
- TensorFlow / Keras
- Flask (ou FastAPI)
- HTML/CSS (frontend)
- Docker
- Jenkins
- Kubernetes (GKE Autopilot)

## 📄 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE)  file for more details.

## 🤝 Contribution

Contributions are welcome! Feel free to open issues and pull requests.
