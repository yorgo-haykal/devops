# Base imahe Java 21 JDK
FROM eclipse-temurin:21-jdk-alpine AS myapp-build

# Sets a reusable env var for the app's folder
ENV APP_HOME=/opt/myapp
# Change the current directory for the rest of the commands
WORKDIR $APP_HOME

# Install Maven into the build stage
RUN apk add --no-cache maven

# Copy the pom into the image
COPY simpleapi/pom.xml .
# Copy the app source code into the image
COPY simpleapi/src ./src
# Run Maven to resolve dependencies and build the project
RUN mvn package -DskipTests

# Base imahe Java 21 JRE
FROM eclipse-temurin:21-jre-alpine
ENV APP_HOME=/opt/myapp
WORKDIR $APP_HOME
# Copy the jar into the runtime stage
COPY --from=myapp-build /opt/myapp/target/*.jar ./myapp.jar

# Define the default command: start the app with JRE
ENTRYPOINT ["java", "-jar", "myapp.jar"]