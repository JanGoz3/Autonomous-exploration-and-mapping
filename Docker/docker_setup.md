## ROS 2 & Unity Docker Workflow

### 1. One-Time Setup (Build Image & Persistent Container)

Run these commands once from inside the `docker/` directory:

1. **Build the Docker image:**
   ```bash
   docker build -t foxy .
   ```

2. **Create and start the persistent container:**
   ```bash
   docker run -dit --name ros2-unity -p 10000:10000 foxy /bin/bash
   

---

### 2. Starting the ROS–Unity Bridge

1. Open **Docker Desktop → Containers** and click **Start (▶)** next to `ros2-unity`.
2. Click `ros2-unity` and open the **Exec** tab.
3. Switch from `/bin/sh` to `bash`, source ROS 2 Foxy and the workspace, and launch the endpoint server:
   ```bash
   bash
   source /opt/ros/foxy/setup.bash
   source /home/dev_ws/install/setup.bash
   ros2 run ros_tcp_endpoint default_server_endpoint --ros-args -p ROS_IP:=0.0.0.0
   ```
4. Leave this tab open while running Unity in **Play** mode (Unity connects on `127.0.0.1:10000`).

---

### 3. Inspecting & Streaming Live ROS 2 Topics

While the endpoint server runs in Docker Desktop, open a separate terminal (e.g., in VS Code) to inspect messages:

1. **Attach a second shell to the running container and load the environment:**
   ```bash
   docker exec -it ros2-unity bash
   source /opt/ros/foxy/setup.bash
   source /home/dev_ws/install/setup.bash
   ```

2. **Inspect topics:**
   * **List active topics:**
     ```bash
     ros2 topic list
     ```
   * **Stream live messages** (press `Ctrl + C` to stop):
     ```bash
     ros2 topic echo /pos_rot
     ```
   * **Print a single message snapshot:**
     ```bash
     ros2 topic echo /pos_rot --once
     ```
   * **Check publishing rate (Hz):**
     ```bash
     ros2 topic hz /pos_rot
     ```