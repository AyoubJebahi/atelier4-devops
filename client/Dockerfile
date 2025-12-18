FROM gcc:latest

WORKDIR /app

COPY client.c .

RUN gcc -o client client.c

# On met pour l'instant 127.0.0.1:6000 (on changera pour Kubernetes plus tard)
CMD ["./client", "127.0.0.1", "6000"]
