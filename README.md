# py-computabl
A Python Implementation of the Computabl SDK

#### Submit a job running in a podman container to a cluster
```python
import computabl

# Create a connection to a computabl backend
conn = computabl.Connection(token='ACCESS_TOKEN', endpoint='computabl.cluster.example.com')

# Create a job
job = conn.create_job(name='Hello World', type='podman', num_cpu=1, wall_time='5m')

# Initialize a new volume
input_volume = conn.create_volume('hello-input')

# Sync files between your local computer and the new volume
input_volume.sync_directory('input-greetings/')

# Attach the volume to the your new job
job.volumes.append(input_volume)

# Create a result volume and attach it to your job
output_volume = conn.create_volume('hello-out')
job.volumes.append(output_volume)

# Set the container identifier for the podman job
job.envrionment['CONTAINER_URI'] = 'ghcr.io/autamus/hello-world:latest'
job.environment['CONTAINER_CMD'] = '/bin/greet --input-dir="/job/hello-input/" --output-dir="/job/hello-out/"'

# Submit the job
job.submit(notify="user@email.com")
```

#### Submit a non-container job to a cluster
```python
import computabl

# Create a connection to a computabl backend
conn = computabl.Connection(token='ACCESS_TOKEN', endpoint='computabl.cluster.example.com')

# Create a job
job = conn.create_job(name='Hello World', type='default', num_cpu=1, wall_time='5m')

# Initialize a new volume
input_volume = conn.create_volume('hello-input')

# Sync files between your local computer and the new volume
input_volume.sync_directory('input-greetings/')

# Attach the volume to the your new job
job.volumes.append(input_volume)

# Create a result volume and attach it to your job
output_volume = conn.create_volume('hello-out')
job.volumes.append(output_volume)

# Load the required modules into the job
job.modules.append("python/3.9")
job.modules.append("cowsay")

# Set the command for the job
job.environment['CMD'] = 'cowsay --input-dir="/job/hello-input/" --output-dir="/job/hello-out/"'

# Submit the job
job.submit(notify="user@email.com")
```

#### Sync the results to your computer
```python
import computabl

# Create a connection to the computabl backend
conn = computabl.Connection(token='ACCESS_TOKEN', endpoint='computabl.cluster.example.com')

# Open an existing volume
output_volume = conn.open_volume('hello-out')

# Sync files from the output volume to your local computer
output_volume.sync_directory('greeting-output/')

```
