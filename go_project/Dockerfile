# Используем образ golang для сборки нашего приложения
FROM golang:1.16 AS builder

# Установка рабочей директории внутри контейнера
WORKDIR /app

# Копируем go.mod и go.sum для загрузки зависимостей
COPY go.mod .
COPY go.sum .

# Загружаем зависимости
RUN go mod download

# Копируем остальной код приложения
COPY . .

# Собираем приложение
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .

# Используем образ alpine в качестве базового образа для уменьшения размера образа
FROM alpine:latest  

# Устанавливаем необходимые пакеты
RUN apk --no-cache add ca-certificates

# Устанавливаем рабочую директорию
WORKDIR /root/

# Копируем исполняемый файл из предыдущего этапа
COPY --from=builder /app/main .

# Устанавливаем права на исполнение
RUN chmod +x ./main

# Команда для запуска приложения
CMD ["./main"]
