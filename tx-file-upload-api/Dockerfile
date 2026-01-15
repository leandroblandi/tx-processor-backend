# Use an official Java runtime as a parent image
FROM amazoncorretto:21.0.4-alpine3.18

# Set the working directory in the container
WORKDIR /app

# Copy the JAR file into the container
COPY target/txfu.jar app.jar

# Expose the port that the application will run on
EXPOSE 8082

# Run the JAR file
ENTRYPOINT ["java", "-XX:MaxRAMPercentage=75", "-jar", "app.jar"]