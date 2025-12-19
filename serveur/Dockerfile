# --- ÉTAPE 1: Build Stage (Compilation) ---
FROM gcc:11 AS build
WORKDIR /app
COPY . .
# Supposons que votre programme est compilé avec gcc -o serveur
RUN gcc -o serveur serveur.c

# --- ÉTAPE 2: Final Stage (Image Légère) ---
FROM alpine:latest
WORKDIR /usr/local/bin
# Copie uniquement le binaire compilé
COPY --from=build /app/serveur .
# Spécifiez la commande d'exécution
CMD ["./serveur", "6000"]
