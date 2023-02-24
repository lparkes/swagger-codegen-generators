FROM golang:1.20 AS build
WORKDIR /go/src
COPY . .

# If you have set -DpackageName when running swagger-codegen, then
# replace "swagger" with your package name.
RUN go mod init swagger
RUN go mod tidy

# The combination of cgo and net/http causes go to build a shared
# exectable. By turning off cgo we get a fully static executable which
# works much more easily in a container.
ENV CGO_ENABLED=0
RUN go build extra/main.go

FROM scratch AS runtime
COPY --from=build /go/src/main ./
EXPOSE 8080/tcp
ENTRYPOINT ["./main"]
