FROM golang:1.24.3 AS build
WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o /output/backend ./cmd/api/

FROM alpine:3.21.3
RUN apk add --no-cache curl
RUN adduser -D -u 1000 appuser
USER appuser
COPY --from=build /output/backend /usr/local/bin/backend
ENTRYPOINT ["/usr/local/bin/backend"]
