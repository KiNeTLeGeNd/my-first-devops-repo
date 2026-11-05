# --- ЭТАП 1: СБОРКА ---
FROM golang:1.22 AS builder
WORKDIR /build

# Копируем всё содержимое папки backend
COPY backend/ .

# Принудительно скачиваем нужную библиотеку (обходим проблему с go.sum)
RUN go get github.com/lib/pq

# Указываем, что это модуль, и собираем бинарник
RUN go mod init app 2>/dev/null || true
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o /out/app ./main.go

# --- ЭТАП 2: ЗАПУСК ---
FROM gcr.io/distroless/static-debian12:nonroot
WORKDIR /app
COPY --from=builder /out/app /app/app
COPY frontend/ /app/static/

ENTRYPOINT ["/app/app"]