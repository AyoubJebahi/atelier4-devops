FROM gcc:11

WORKDIR /app

# Copier le code source
COPY . .

# Compiler le serveur
RUN gcc -o serveur serveur.c

# Port écouté par le serveur
EXPOSE 6000

# Commande de démarrage
CMD ["./serveur"]

