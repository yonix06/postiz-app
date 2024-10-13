# -*- mode: Python -*-

# Enforce a minimum Tilt version, so labels are supported //TODO 3 - voir TO DO 2
# https://docs.tilt.dev/api.html#api.version_settings
version_settings(constraint='>=0.22.1')

docker_compose('docker-compose.dev.yml')

docker_build(
  # Image name - must match the image in the docker-compose file
  'localhost/postiz-devcontainer',
  # Docker context
  '.',
  live_update = [
    # Sync local files into the container.
    sync('.', '/app'),
    sync('libraries', '/app/libraries'),
    sync('/apps', '/app/apps'),
    sync('nx.json', '/app/nx.json'),
    sync('tsconfig.base.json', '/app/tsconfig.base.json'),
    sync('package.json', '/app/package.json'),
    sync('package-lock.json', '/app/package-lock.json'),
    sync('/var/docker/entrypoint.sh', '/app/entrypoint.sh'),
    sync('/var/docker/supervisord.conf', '/etc/supervisord.conf'),
    sync('/var/docker/supervisord', '/app/supervisord_available_configs/'),
    sync('/var/docker/Caddyfile', '/app/Caddyfile'),
    sync('.env', '/config/postiz.env'),


    # Re-run npm install whenever package.json changes.
    run('npm i', trigger='package.json'),

    # Restart the process to pick up the changed files.
    restart_container()
  ])

# Add labels to Docker services
dc_resource('redis', labels=["database"])
dc_resource('app', labels=["server"])