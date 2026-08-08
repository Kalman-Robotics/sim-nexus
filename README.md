# Simulación NEXUS

Simulación en Gazebo del robot **NEXUS** (Kit Kalman) para **ROS 2 Humble**, usada en los cursos de Kalman Robotics.

<img src="images/laboratorio_robotica.png" alt="NEXUS en el laboratorio" width="640" />

- [Simulación NEXUS](#simulación-nexus)
  - [Requisitos](#requisitos)
  - [Instalación](#instalación)
  - [Uso](#uso)
  - [Referencias](#referencias)

## Requisitos

- Ubuntu 22.04 con ROS 2 Humble instalado.

## Instalación

**1. Crea el workspace de simulación con su carpeta `src` (si ya lo tienes, ve al paso 2):**

```bash
mkdir -p ~/sim_ws/src
```

**2. Clona este repositorio dentro de `src`:**

```bash
cd ~/sim_ws/src
git clone https://github.com/Kalman-Robotics/sim-nexus.git
```

**3. Descarga las dependencias con rosdep:**

Si es la primera vez que usas `rosdep` en tu máquina, inicialízalo:

```bash
sudo rosdep init
rosdep update
```

Luego, desde la raíz del workspace, instala todo lo que los paquetes necesitan (Gazebo incluido):

```bash
cd ~/sim_ws
rosdep install --from-paths src --ignore-src -r -y
```

**4. Compila el workspace y actívalo:**

```bash
colcon build
source install/setup.bash
```

## Uso

Verifica que los paquetes estén disponibles:

```bash
ros2 pkg list | grep kalman
```

Lanza la simulación:

```bash
ros2 launch kalman_gazebo simulation.launch.py
```

Puedes elegir el mundo con el argumento `world`:

```bash
ros2 launch kalman_gazebo simulation.launch.py world:=laboratorio.world
ros2 launch kalman_gazebo simulation.launch.py world:=vacio.world
```

Para cerrar la simulación, presiona `Ctrl+C` en la terminal.

## Referencias

- [Kit Kalman ROS 2 (robot real)](https://github.com/Kalman-Robotics/kit-kalman-ros2)
- [Kalman Robotics](https://kalmanrobotics.io/)
