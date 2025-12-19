FROM gcc:11

WORKDIR /app

# Copier le code source
COPY . .

# Compiler le client
RUN gcc -o client client.c

# ⚠️ Très important : dans Kubernetes, le client ne se connecte PAS à 127.0.0.1
# mais au service "serveur-service" défini dans k8s-serveur-deployment.yaml
CMD ["./client", "serveur-service", "6000"]

