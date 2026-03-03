# ---------- Stage 1 ----------
FROM golang:1.22 AS builder

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN CGO_ENABLED=0 go build -o go-task-api

# ---------- Stage 2 ----------
FROM ubuntu:22.04

WORKDIR /app

RUN apt-get update && apt-get install -y ca-certificates && rm -rf /var/lib/apt/lists/*

COPY --from=builder /app/go-task-api /app/go-task-api

RUN chmod +x /app/go-task-api

EXPOSE 8080
ENTRYPOINT ["/app/go-task-api"]