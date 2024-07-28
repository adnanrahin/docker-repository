1. Build Image `docker build -t airflow:latest -f Dockerfile .`
2. Spawn up the container `docker run -d --name airflow -v /Users/adnanrahin/docker-share/airflow:/root/airflow -p 38080:18080 airflow:latest`
