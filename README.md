# Build Image
Open a terminal
```bash
# Clone the repo
git clone https://github.com/ibm-messaging/mq-metric-samples.git

cd mq-metric-samples

# Build the image
docker build -t mqprom:1.0 .

# Run the container
docker run --rm -it --name mqprom-test mqprom:1.0 sh 
```

# Run the container then copy the binary and shared libs into local machine
Open another terminal
```bash
# Create a new dir
mkdir mq-prome-binary
cd mq-prome-binary

# Copy binary and shared libs from a running container
docker cp mqprom-test:/opt/bin/mq_prometheus .
docker cp mqprom-test:/opt/mqm ./mqm
# Copy global config template from repo folder
cp ../mq-metric-samples/config.common.yaml mq_prometheus.yaml
```

# Run the binary outside the container
Modify `mq_prometheus.yaml`

From this [`Dockerfile`](https://github.com/ibm-messaging/mq-metric-samples/blob/master/Dockerfile) we need some env vars, `LD_LIBRARY_PATH`, `MQ_CONNECT_TYPE`, and `IBMMQ_GLOBAL_CONFIGURATIONFILE`

Run the binary with env vars
```bash
LD_LIBRARY_PATH="./mqm/lib64:/usr/lib64" MQ_CONNECT_TYPE=CLIENT IBMMQ_GLOBAL_CONFIGURATIONFILE=./mq_prometheus.yaml ./mq_prometheus 
```
