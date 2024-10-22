# Strix Dev Docker

## GTK Broadway
Adds browser display capability to GTK applications such as GTKWave

You can start the broadway server with this command:
```bash
broadwayd :5 &
```

Once the broadway server is running, start the gtkwave application with this command:
```bash
GDK_BACKEND=broadway BROADWAY_DISPLAY=:5 gtkwave
```

Alternatively there is script at `/usr/local/share/gui.sh` that will run the above commands.

Then you can open your browser to 'localhost:8085' and see the GTKWave gui in the browser window.

## Devcontiner
Add the contents of this folder to a folder in your project called `.devcontainer`