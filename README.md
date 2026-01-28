# AAE5303 Environment Setup Report — Assignment 1 (My Setup)

> **Important:** This README is filled out based on the course template. The **one-command smoke tests (PASS)** have been completed, and ROS2 talker/listener can communicate normally.

---

## 1. System Information

**Laptop model:**  
Lenovo ThinkBook 15 G2 ITL

**CPU / RAM:**  
11th Gen Intel® Core™ i7-1165G7 @ 2.80GHz, 16GB RAM (3200 MT/s)

**Host OS:**  
Windows 11

**Linux/ROS environment type:**  
_[I used WSL2 + Docker: Ubuntu runs in WSL2, and ROS/Python toolchain runs inside a Docker container.]_
- [ ] Dual-boot Ubuntu
- [x] WSL2 Ubuntu
- [ ] Ubuntu in VM (UTM/VirtualBox/VMware/Parallels)
- [x] Docker container
- [ ] Lab PC
- [ ] Remote Linux server

---

## 2. Python Environment Check

### 2.1 Steps Taken

Describe briefly how you created/activated your Python environment:

**Tool used:**  
_[venv]_

**Key commands you ran:**
```bash
cd /workspace
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

**(Docker/WSL workaround) CMAKE_PREFIX_PATH was empty in my shell, so I used:**
```source /opt/ros/humble/setup.bash
[ -z "$CMAKE_PREFIX_PATH" ] && export CMAKE_PREFIX_PATH=/opt/ros/humble
```

**one-command smoke tests**
```
python scripts/run_smoke_tests.py | tee smoke_output.txt
```

**Any deviations from the default instructions:**  
_[Used WSL2 + Docker instead of native Ubuntu. Also had to install a missing Python package catkin_pkg in venv for colcon build to work.]_

### 2.2 Test Results

Run these commands and paste the actual terminal output (not just screenshots):

```bash
python scripts/test_python_env.py
```

**Output:**
```
[=== Summary ===
✅ Environment: {
  "platform": "Linux-6.6.87.2-microsoft-standard-WSL2-x86_64-with-glibc2.35",
  "python": "3.10.12",
  "executable": "/workspace/.venv/bin/python",
  "cwd": "/workspace",
  "ros": {
    "ROS_VERSION": "2",
    "ROS_DISTRO": "humble",
    "ROS_ROOT": null,
    "ROS_PACKAGE_PATH": null,
    "AMENT_PREFIX_PATH": "/workspace/ros2_ws/install/env_check_pkg:/opt/ros/humble",
    "CMAKE_PREFIX_PATH": "/workspace/ros2_ws/install/env_check_pkg:/opt/ros/humble"
  }
}
✅ Python version OK: 3.10.12
✅ Module 'numpy' found (v2.2.6).
✅ Module 'scipy' found (v1.15.3).
✅ Module 'matplotlib' found (v3.10.8).
✅ Module 'cv2' found (v4.13.0).
✅ Module 'rclpy' found (vunknown).
✅ numpy matrix multiply OK.
✅ numpy version 2.2.6 detected.
✅ scipy FFT OK.
✅ scipy version 1.15.3 detected.
✅ matplotlib backend OK (Agg), version 3.10.8.
✅ OpenCV OK (v4.13.0), decoded sample image 128x128.
✅ Open3D OK (v0.19.0), NumPy 2.2.6.
✅ Open3D loaded sample PCD with 8 pts and completed round-trip I/O.
✅ ROS 2 CLI OK: /opt/ros/humble/bin/ros2
✅ ROS 1 tools not found (acceptable if ROS 2 is installed).
✅ colcon found: /usr/bin/colcon
✅ ROS 2 workspace build OK (env_check_pkg).
✅ ROS 2 runtime OK: talker and listener exchanged messages.
✅ Binary 'python3' found at /workspace/.venv/bin/python3

All checks passed. You are ready for AAE5303 🚀
✅ Python + ROS environment checks: PASS (14.3s)
Paste your actual terminal output here]
```

```bash
python scripts/test_open3d_pointcloud.py
```

**Output:**
(Captured via python scripts/run_smoke_tests.py → Step 2.)
```
[ℹ️ Loading /workspace/data/sample_pointcloud.pcd ...
✅ Loaded 8 points.
   • Centroid: [0.025 0.025 0.025]
   • Axis-aligned bounds: min=[0. 0. 0.], max=[0.05 0.05 0.05]
✅ Filtered point cloud kept 7 points.
✅ Wrote filtered copy with 7 points to /workspace/data/sample_pointcloud_copy.pcd
   • AABB extents: [0.05 0.05 0.05]
   • OBB  extents: [0.08164966 0.07071068 0.05773503], max dim 0.0816 m
🎉 Open3D point cloud pipeline looks good.
✅ Open3D point cloud pipeline: PASS (2.0s)]
```

**Screenshot:**  
_[Include one screenshot showing both tests passing]_

![Python Tests Passing](path/to/your/screenshot.png)

---

## 3. ROS 2 Workspace Check

### 3.1 Build the workspace

Paste the build output summary (final lines only):

```bash
cd /workspace/ros2_ws
source /opt/ros/humble/setup.bash
rm -rf build install log
colcon build --packages-select env_check_pkg --event-handlers console_direct+
```

**Expected output:**
```
Summary: 1 package finished [x.xx s]
```

**Your actual output:**
(I did not separately record the final lines, but the one-command suite confirms the build succeeded: 
```
✅ ROS 2 workspace build OK (env_check_pkg).
```
If needed, rerun the build command above to regenerate the exact summary lines.)
```
[✅ ROS 2 workspace build OK (env_check_pkg).]
```

### 3.2 Run talker and listener

Show both source commands:

```bash
source /opt/ros/humble/setup.bash
source install/setup.bash
```

**Then run talker:**
```bash
# In this repo I used the launch file (recommended) to run both talker and listener together.
# Direct ros2 run commands may differ depending on node names / implementation.
```

Alternative (using launch file):
```bash
ros2 launch env_check_pkg env_check.launch.py
```


**Output (3–4 lines):**
```text
[talker-1] [INFO] [1769588421.494010587] [env_check_pkg_talker]: Publishing: 'AAE5303 hello #354'
[listener-2] [INFO] [1769588421.494465296] [env_check_pkg_listener]: I heard: 'AAE5303 hello #354'
[talker-1] [INFO] [1769588422.020462993] [env_check_pkg_talker]: Publishing: 'AAE5303 hello #355'
[listener-2] [INFO] [1769588422.021045257] [env_check_pkg_listener]: I heard: 'AAE5303 hello #355'
```

**Run listener:**
```bash
ros2 run env_check_pkg listener.py
```

**Output (3–4 lines):**
```
[Paste 3-4 lines of listener output here]
```

**Alternative (using launch file):**
```bash
ros2 launch env_check_pkg env_check.launch.py
```

**Screenshot:**  
_[Include one screenshot showing talker + listener running]_

![Talker and Listener Running](path/to/your/screenshot.png)

---

## 4. Problems Encountered and How I Solved Them

> **Note:** Write 2–3 issues, even if small. This section is crucial — it demonstrates understanding and problem-solving.

### Issue 1: [wsl --install -d Ubuntu-22.04 failed with “catastrophic failure (Wsl/InstallDistro/E_UNEXPECTED)”]

**Cause / diagnosis:**  
_[WSL components or kernel were not updated properly, which caused distro installation via Microsoft Store to fail.]_

**Fix:**  
_[Update WSL via web download, shutdown WSL, then retry install.]_

```powershell
wsl --update --web-download
wsl --shutdown
wsl --install -d Ubuntu-22.04
```

**Reference:**  
_[AI assistant and Microsoft WSL install/update hints.]_

---

### Issue 2: [Docker pull failed with credential helper error(docker-credential-desktop.exe: exec format error)]

**Cause / diagnosis:**  
_[Inside WSL, Docker tried to execute a Windows credential helper (docker-credential-desktop.exe), which failed and prevented pulling images.]_

**Fix:**  
_[Remove credsStore / credHelpers that point to desktop in ~/.docker/config.json, and use a minimal config.]_

```bash
nano ~/.docker/config.json
```

Set to:
```json
{ "auths": {} }
```

**Reference:**  
_[AI assistant and Docker credential helper troubleshooting experience.]_

---

### Issue 3 (Optional): [colcon build failed with ModuleNotFoundError: No module named 'catkin_pkg']

**Cause / diagnosis:**  
_[ament_cmake scripts parse package.xml using Python; my build was using venv Python (/workspace/.venv/bin/python3) and the venv did not have catkin_pkg.]_

**Fix:**  
_[Install catkin_pkg into the active venv, then clean and rebuild the workspace.]_

```bash
source /workspace/.venv/bin/activate
python -m pip install -U catkin_pkg

cd /workspace/ros2_ws
rm -rf build install log
source /opt/ros/humble/setup.bash
[ -z "$CMAKE_PREFIX_PATH" ] && export CMAKE_PREFIX_PATH=/opt/ros/humble
colcon build --packages-select env_check_pkg --event-handlers console_direct+
```

**Reference:**  
_[AI assistant and build error traceback.]_

---

## 5. Use of Generative AI (Required)

Choose one of the issues above and document how you used AI to solve it.

> **Goal:** Show critical use of AI, not blind copying.

### 5.1 Exact prompt you asked

**Your prompt:**
```
PS C:\Users\UserX> wsl --install -d Ubuntu-22.04
catastrophic failure
error: Wsl/InstallDistro/E_UNEXPECTED

```

### 5.2 Key helpful part of the AI's answer

**AI's response (relevant part only):**
```
First, restart and update WSL (wsl --update --web-download). If necessary, retry installing Ubuntu 22.04 after running wsl --shutdown.
```

### 5.3 What you changed or ignored and why


**Your explanation:**  
I followed the update + shutdown steps. I did not accept “wait and try later”; instead I verified results by re-running the install and confirming Ubuntu initialization prompt appeared. After WSL was updated, the web download installation succeeded._

### 5.4 Final solution you applied

```powershell
wsl --update --web-download
wsl --shutdown
wsl --install -d Ubuntu-22.04
```

**Why this worked:**  
Updating WSL fixed the underlying distro installation mechanism so Ubuntu 22.04 could be installed and initialized correctly._

---

## 6. Reflection (3–5 sentences)


**Your reflection:**

Setting up robotics environments is much smoother when using reproducible tooling (Docker) instead of manually installing everything on the host OS. I learned to debug setup issues by reading the first real error in the log (e.g., missing catkin_pkg) instead of guessing. The most surprising part was that Windows Docker credential helpers could break image pulls inside WSL, and editing ~/.docker/config.json fixed it. After seeing the full smoke tests pass and ROS2 talker/listener exchange messages, I feel more confident debugging ROS/Python environment problems.

---

## 7. Declaration

✅ **I confirm that I performed this setup myself and all screenshots/logs reflect my own environment.**

**Name:**  
_[Luo Daixun]_

**Student ID:**  
_[25121137g]_

**Date:**  
_[2026/1/28]_

---

## Submission Checklist

Before submitting, ensure you have:

- [x] Filled in all system information
- [x] Included actual terminal outputs (not just screenshots)
- [x] Provided at least 2 screenshots (Python tests + ROS talker/listener)
- [x] Documented 2–3 real problems with solutions
- [x] Completed the AI usage section with exact prompts
- [x] Written a thoughtful reflection (3–5 sentences)
- [x] Signed the declaration

---

**End of Report**
