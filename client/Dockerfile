# --- ÉTAPE 1: Build Stage (Compilation) ---
FROM gcc:11 AS build
WORKDIR /app
COPY . .
# Supposons que votre programme est compilé avec gcc -o client
RUN gcc -o client client.c

# --- ÉTAPE 2: Final Stage (Image Légère) ---
FROM alpine:latest
WORKDIR /usr/local/bin
# Copie uniquement le binaire compilé
COPY --from=build /app/client .
# Spécifiez la commande d'exécution.
# L'hôte sera 'serveur-service', nom DNS K8s.
CMD ["./client", "serveur-service", "6000"]
