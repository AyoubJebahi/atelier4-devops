FROM gcc:latest

WORKDIR /app

COPY serveur.c .

RUN gcc -o serveur serveur.c

CMD ["./serveur"]
