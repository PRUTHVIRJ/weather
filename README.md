### overveiw
Near-Real-Time Weather Data Processing
In this project, we embark on an exploratory journey through various technologies, from back-end systems to front-end interfaces. Our primary objective isn't solely the weather data itself but to gain foundational knowledge in near-real-time data processing.

### Key Components
Kappa Architecture: Our backbone for this project is the Kappa Architecture, which provides a flexible, fault-tolerant, and scalable framework for processing substantial data streams. While typically used for real-time data, we're adapting it for near-real-time processing of weather information, acknowledging that such data may not be perfectly accurate.

### Open Weather APIs:
Geocoding API: This tool from OpenWeather simplifies location searches using geographic names. We utilize this API to retrieve the latitude and longitude of user-specified cities, paving the way for precise weather data retrieval.
Weather Current Data API: Access current weather data from any location on Earth! OpenWeather aggregates and processes weather information from various sources, including global and local weather models, satellites, radars, and an extensive network of weather stations. The data is available in versatile formats such as JSON, XML, or HTML.
EC2 Instance: Leveraging Amazon Elastic Compute Cloud (EC2), part of Amazon Web Services (AWS), we set up a virtual machine running Ubuntu to host our back-end code, providing the computational power needed for our application.

AWS Gateway API: This AWS service facilitates the creation, publishing, maintenance, monitoring, and securing of RESTful, HTTP, and WebSocket APIs at any scale. We employ it to secure the connection between our VM and the front-end, which is hosted on GitHub Pages.

Angular: As a TypeScript-based framework for single-page web applications, Angular serves as the bridge between our Spring Boot API and the user interface, ensuring smooth communication and a seamless user experience.

Kafka, Spark, and Cassandra: This trio forms the heart of our near-real-time data processing efforts, allowing us to efficiently handle and analyze incoming weather data streams.

Dashboard Update: We're currently working on enhancing our dashboard feature. Although it's not supported in version 1.0.1, we are creatively addressing this to deliver an engaging and informative user interface.

### Developer Guide:
In order to run the code on your local machine you can clone the repository:
```bash
git clone https://github.com/PRUTHVIRJ/weather.git
cd weather
```
we need some requirements `Zookeeper`, `Kafka`, `Spark` and `Cassandra` you can get them by running the docker compose file:
```bash
sudo docker-compose up -d
```
the **-d** just to not display the logs so you don't have to run another terminal.

For the spark container you can run this command:
```bash
sudo docker run --name spark-container --net put-the-same-network-name-that-the-services-running-in -it apache/spark:v3.2.3 /opt/spark/bin/spark-shell
```

if you don't know the network that the services are using you can run:
```bash
sudo docker inspect kafka-or-cassandra-container-name-just-one-of-them
```

To check that the the services are running use:
```bash
sudo docker ps
```
They should be up and running.

Now, you can choose to run the code by yourself or create a docker image from the app. here is how you can do it:
```bash
sudo docker build -t name-your-image:tag-image .
```
We told docker to create an image from a dockerfile exist in the current directory .

Now, lets create a container for our app:
```bash
sudo docker run --name weather-api-v1 --net put-the-same-network-name-that-the-services-running-in -p 8080:8080 name-your-image:tag-image
```
Now, you can access the app over: **http://localhost:8080**

- if you want to change the varibale envirements you can change them use **-e** tag in running command. here is the varibales that you can adjust:
  ```bash
    CASSANDRA_CONTACT_POINTS=cassandra
    CASSANDRA_KEYSPACE=weather
    CASSANDRA_PORT=9042
    
    KAFKA_BOOTSTRAP_SERVERS=localhost
    KAFKA_PORT=29092
    
    SPARK_MASTER=local[*]
  ```
  you can do it like this:
  ```bash
  sudo docker run --name weather-api-v1 --net put-the-same-network-name-that-the-services-running-in -e CASSANDRA_CONTACT_POINTS=new_cassandra -e KAFKA_PORT=new_port -p 8080:8080 name-your-image:tag-image
  ```
- The last thing don't forget to create a keyspace with same name in your cassandra container:
  ```bash
    sudo docker exec -it cassandra-container-ip cqlsh
  ```
  Now create it:
  ```bash
  CREATE KEYSPACE IF NOT EXISTS weather
    WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};
  ```
