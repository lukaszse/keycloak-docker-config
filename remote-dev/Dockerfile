FROM quay.io/keycloak/keycloak:latest as builder

ENV KC_HEALTH_ENABLED=true
ENV KC_METRICS_ENABLED=true
ENV KC_FEATURES=token-exchange
ENV KC_DB=postgres

# Install custom providers
RUN curl -sL https://github.com/aerogear/keycloak-metrics-spi/releases/download/2.5.3/keycloak-metrics-spi-2.5.3.jar -o /opt/keycloak/providers/keycloak-metrics-spi-2.5.3.jar
RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:latest
COPY --from=builder /opt/keycloak/ /opt/keycloak/
WORKDIR /opt/keycloak

RUN keytool -genkeypair -storepass password -storetype PKCS12 -keyalg RSA -keysize 2048 -dname "CN=Keycloak,O=UŚ,L=Sosnowiec,C=Poland" -alias server -ext "SAN:c=DNS:ubuntudev.llseremak.p5.tiktalik.io,IP:37.233.102.95" -keystore conf/server.keystore changeit -startdate "2022/11/19 00:00:00" -validity 365

# Change these values to point to a running postgres instance
ENV KC_DB_URL=jdbc:postgresql://postgres:5432/keycloak
ENV KC_DB_USERNAME=keycloak
ENV KC_DB_PASSWORD=Passw0rd
ENV KC_HOSTNAME=37.233.102.95

ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start-dev"]