# ---------- Stage 1: Build ----------
FROM ubuntu:22.04 AS builder
RUN apt-get update&& apt-get install -y golang-go git build-essential && rm -rf /var/lib/apt/lists/*
WORKDIR /app
COPY go.mod go.sum ./ 
RUN go mod download
COPY go-task-api.go/ /app/
RUN go build -o go-task-api go-task-api.go
# ---------- Stage 2: Run ----------
FROM scratch
WORKDIR /app
COPY --from=builder /app/go-task-api /app
EXPOSE 8080
ENTRYPOINT ["./go-task-api"]
