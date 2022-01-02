# py-computabl
A Python Implementation of the Computabl SDK

#### Submit a job to a cluster
```python
import computabl

# Create a connection to a computabl backend
conn = computabl.Connection(token='ACCESS_TOKEN', endpoint='computabl.example.com')

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
job.submit(notify="hi@alecbcs.com")
```

#### Sync the results to your computer
```python
import computabl

# Create a connection to the computabl backend
conn = computabl.Connection(token='ACCESS_TOKEN', endpoint='computabl.example.com')

# Open an existing volume
output_volume = conn.open_volume('hello-out')

# Sync files from the output volume to your local computer
output_volume.sync_directory('greeting-output/')

```
