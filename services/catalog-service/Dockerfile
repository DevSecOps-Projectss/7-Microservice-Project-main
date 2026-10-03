# ---------- Build stage ----------
FROM golang:1.22-alpine AS build
WORKDIR /src

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -trimpath -ldflags="-s -w" -o /out/catalog-service .

# ---------- Runtime stage ----------
FROM alpine:3.20
WORKDIR /app

RUN apk add --no-cache ca-certificates && \
    addgroup -S appgroup && adduser -S appuser -G appgroup

COPY --from=build --chown=appuser:appgroup /out/catalog-service /app/catalog-service

USER appuser
EXPOSE 8082
ENTRYPOINT ["/app/catalog-service"]
